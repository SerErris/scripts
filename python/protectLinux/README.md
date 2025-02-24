# Add Physical Linux Servers to File-based Protection Job Using Python

Warning: this code is provided on a best effort basis and is not in any way officially supported or sanctioned by Cohesity. The code is intentionally kept simple to retain value as example code. The code in this repository is provided as-is and the author accepts no liability for damages resulting from its use.

This script adds physical linux servers to a file-based protection job.

Note: this script is written for Cohesity 6.5.1 and later

## Download the script

You can download the scripts using the following commands:

```bash
# download commands
curl -O https://raw.githubusercontent.com/bseltz-cohesity/scripts/master/python/protectLinux/protectLinux.py
curl -O https://raw.githubusercontent.com/bseltz-cohesity/scripts/master/python/pyhesity.py
chmod +x protectLinux.py
# end download commands
```

## Components

* protectLinux.py: the main powershell script
* pyhesity.py: the Cohesity REST API helper module

Place both files in a folder together and run the main script like so:

```bash
./protectLinux.py -v mycluster \
                  -u myuser \
                  -d mydomain.net \
                  -j 'My Backup Job' \
                  -s myserver.mydomain.net \
                  -l serverlist.txt \
                  -i /var \
                  -i /home \
                  -n includes.txt \
                  -e /var/log \
                  -e /home/oracle \
                  -x excludes.txt
```

## Parameters

* -v, --vip: DNS or IP of the Cohesity cluster to connect to
* -u, --username: username to authenticate to Cohesity cluster
* -d, --domain: (optional) domain of username, defaults to local
* -k, --useApiKey: (optional) use API key for authentication
* -pwd, --password: (optional) password of API key
* -s, --servername: (optional) name of server to add to the job (use multiple times for multiple)
* -l, --serverlist: (optional) list of server names in a text file
* -j, --jobname: name of the job to add the server to
* -a, --alllocaldrives: (optional) protect all local drives
* -mf, --metadatafile: (optional) metadata file path
* -i, --include: (optional) file path to include (use multiple times for multiple paths)
* -n, --includefile: (optional) a text file full of include paths
* -x, --exclude: (optional) file path to exclude (use multiple times for multiple paths)
* -f, --excludefile: (optional) a text file full of exclude file paths
* -m, --skipnestedmountpoints: (optional - 6.3 and below) if omitted, nested mount paths are not skipped
* -t, --skipnestedmountpointtypes: (optional - 6.4 and above) repeat for each mount type to exclude (e.g. -t nfs -t xfs)
* -sd, --storagedomain: (optional) name of storage domain to create job in (default is DefaultStorageDomain)
* -p, --policyname: (optional) name of protection policy to use for new job (only required for new job)
* -tz, --timezone: (optional) time zone for new job (default is US/Eastern)
* -st, --starttime: (optional) start time for new job (default is 21:00)
* -is, --incrementalsla: (optional) incremental SLA minutes (default is 60)
* -fs, --fullsla: (optional) full SLA minutes (default is 120)
* -ei, --enableindexing: (optional) default is no indexing

## Notes

You can specify the names of servers to add on the command line (-s 'server1.mydomain.net' -s 'server2.mydomain.net), or you can point to a text file containing server names (-l serverlist.txt) or both.

**Warning**: If you specify a server that already exists in the job, it's includes and excludes will be overwritten (existing includes and excludes will be lost). Existing servers that are not specified will be left as is.

You can specify exclusions on the command line (-e /var/log -e /home/oracle), or you can point to a text file containing exclusions (-x excludes.txt) or both.
