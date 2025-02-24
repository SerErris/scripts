# Display Job Status Last 24 Hours using Python

Warning: this code is provided on a best effort basis and is not in any way officially supported or sanctioned by Cohesity. The code is intentionally kept simple to retain value as example code. The code in this repository is provided as-is and the author accepts no liability for damages resulting from its use.

This python script reports the status of the last job run of each protection job for the past 24 hours.

## Components

* jobRunReport.py: the main python script
* pyhesity.py: the Cohesity REST API helper module

You can download the scripts using the following commands:

```bash
# download commands
curl -O https://raw.githubusercontent.com/bseltz-cohesity/scripts/master/reports/python/jobRunReport/jobRunReport.py
curl -O https://raw.githubusercontent.com/bseltz-cohesity/scripts/master/python/pyhesity.py
chmod +x jobRunReport.py
# end download commands
```

Place both files in a folder together and run the main script like so:

```bash
bash:~/scripts/python$ ./jobRunReport.py -v mycluster -u myusername
Connected!
             JobName    Status   StartTime
             -------   --------  ---------
          NAS Backup   kSuccess  2019-01-10 00:10:01
          SQL Backup   kSuccess  2019-01-09 23:50:01
      Infrastructure   kSuccess  2019-01-09 23:40:01
      SQL File Based   kSuccess  2019-01-09 20:06:00
          CohesityDB   kSuccess  2019-01-09 23:23:27
   File-Based Backup   kSuccess  2019-01-09 20:12:00
           VM Backup   kSuccess  2019-01-09 23:30:01
              Oracle   kSuccess  2019-01-10 00:00:00
              Isilon   kSuccess  2019-01-09 17:51:39
```

## The Python Helper Module - pyhesity.py

The helper module provides functions to simplify operations such as authentication, api calls, storing encrypted passwords, and converting date formats. The module requires the requests python module.

### Installing the Prerequisites

```bash
sudo yum install python-requests
```

or

```bash
sudo easy_install requests
```
