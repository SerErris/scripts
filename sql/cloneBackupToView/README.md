# Clone Backup Files to a View using PowerShell

Warning: this code is provided on a best effort basis and is not in any way officially supported or sanctioned by Cohesity. The code is intentionally kept simple to retain value as example code. The code in this repository is provided as-is and the author accepts no liability for damages resulting from its use.

This powershell script clones backup files to a Cohesity view.

Note: Consolidation and Refresh options require PowerShell to have SMB access to the Cohesity view.

## Warning! This script can delete views! Make sure you know what you are doing before you run it

## Download the Scripts

Run these commands from PowerShell to download the scripts into the current folder:

```powershell
# Download Commands
$scriptName = 'cloneBackupToView'
$repoURL = 'https://raw.githubusercontent.com/bseltz-cohesity/scripts/master'
(Invoke-WebRequest -Uri "$repoUrl/sql/$scriptName/$scriptName.ps1").content | Out-File "$scriptName.ps1"; (Get-Content "$scriptName.ps1") | Set-Content "$scriptName.ps1"
(Invoke-WebRequest -Uri "$repoUrl/powershell/cohesity-api/cohesity-api.ps1").content | Out-File cohesity-api.ps1; (Get-Content cohesity-api.ps1) | Set-Content cohesity-api.ps1
# End Download Commands
```

## Components

* cloneBackupToView.ps1: the main powershell script
* cohesity-api.ps1: the Cohesity REST API helper module

Place both files in a folder together, then we can run the script.

To clone all available backups:

```powershell
./cloneBackupToView.ps1 -vip mycluster `
                        -username myuser `
                        -domain mydomain.net `
                        -jobName 'My SQL Job' `
                        -objectName mysqlserver.mydomain.net `
                        -viewName cloned
```

To limit access to specific users:

```powershell
./cloneBackupToView.ps1 -vip mycluster `
                        -username myuser `
                        -domain mydomain.net `
                        -jobName 'My SQL Job' `
                        -objectName mysqlserver.mydomain.net `
                        -viewName cloned `
                        -access 'mydomain.net\domain admins', mydomain.net\othergroup
```

To list available runs:

```powershell
./cloneBackupToView.ps1 -vip mycluster `
                        -username myuser `
                        -domain mydomain.net `
                        -jobName 'My SQL Job' `
                        -listRuns
```

To clone a specific range of run dates:

```powershell
./cloneBackupToView.ps1 -vip mycluster `
                        -username myuser `
                        -domain mydomain.net `
                        -jobName 'My SQL Job' `
                        -objectName mysqlserver.mydomain.net `
                        -viewName cloned `
                        -firstRunId 12345 `
                        -lastRunId 12399
```

To delete a view when finished:

```powershell
./cloneBackupToView.ps1 -vip mycluster `
                        -username myuser `
                        -domain mydomain.net `
                        -viewName cloned `
                        -deleteView
```

To refresh a view (on a schedule):

```powershell
./cloneBackupToView.ps1 -vip mycluster `
                        -username myuser `
                        -domain mydomain.net `
                        -jobName 'My SQL Job' `
                        -objectName mysqlserver.mydomain.net `
                        -viewName cloned `
                        -refreshView `
                        -force `
                        -consolidate `
                        -logsOnly
```

## Parameters

* -vip: Cohesity cluster to connect to
* -username: Cohesity username (e.g. admin)
* -domain: (optional) Active Directory domain (defaults to 'local')
* -useApiKey: (optional) use API key for authentication
* -jobname: (optional) name of SQL protection job
* -objectName: (optional) name of sqlServer whose backup to clone
* -viewName: (optional) name of new or existing view to clone backup files to

## Parameters for Run Selection

* -listRuns: (optional) list available job run IDs and dates
* -lastRunOnly: (optional) only clone latest run
* -numRuns: (opyional) max number of runs to clone (default is 100)
* -firstRunId: (optional) earliest run to clone (defaults to all)
* -lastRunId: (optional) most recent run to clone (defaultds to all)

## Parameters for View Manipulation

* -access: (optional) Active Directory users/groups (comma separated) to add to share permissions (default is everyone)
* -deleteView: (optional) delete view when finished
* -refreshView: (optional) delete existing files in view
* -force: (optional) do not prompt for confirmation when refreshing or deleting view (DANGEROUS!)

## Parameters for Folder Manipulation

* -consolidate: (optional) move all files to root of view
* -targetPath: (optional) move all files to this directory (e.g. \test\folder)
* -dbFolders: (optional) move files into db-named folders
* -logsOnly: (optional) only consolidate log files
