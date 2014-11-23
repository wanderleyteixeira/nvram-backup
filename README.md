This script will backup specific [NVRAM](http://en.wikipedia.org/wiki/Non-volatile_random-access_memory “NVRAM”) variables and its values. It is helpful after an upgrade or in the event of problems since Tomato capability to backup and restore is not safe.

I have been using this simple backup script for the past year. It runs every month automatically to backup my selected settings, and generates a script that can be used later to restore configurations. 


Setup
---------------------
* Download the nvram_backup script to your desired backup directory (i.e. CIFS, USB, or NFS). I usually mount a CIFS1 or USB where I keep all backup data, logs, and scripts. 

1. Login to your router
2. Click on **Administration** then **Scripts**
3. Paste the following in the **Init** tab and click to save

```
  REMOTE_PATH="<ADD FULL REMOTE PATH WHERE SCRIPT RESIDES>"
  LOCAL_PATH="/tmp/nvrambackup"
  if [ ! -s "$REMOTE_PATH" ]
    then
    sleep 5
    mkdir $LOCAL_PATH
    cp $REMOTE_PATH/nvram_backup.sh $LOCAL_PATH/nvram_backup.sh
    cp $REMOTE_PATH/nvram_to_keep $LOCAL_PATH/nvram_to_keep
    chmod a+x $LOCAL_PATH/nvram_backup.sh
  fi
```

* Choose the variables you would like to backup and place it on a file called *nvram_to_keep*. You can get a list of all variables and values currently available by ssh’ing to your router and execute the following command:

```  nvram show```

* Run and verify:

```  sh /tmp/nvrambackup/nvram_backup.sh```

* Add cronjob to run at 12:00AM on the first of every month:	

```  cru a backup_nvram "0 0 1 * * /tmp/nvrambackup/nvram_backup.sh"```


Basic Requirements
---------------------
- Router flashed with Tomato Firmware

- Basic Linux knowledge

- A network mounted filesystem

- Tested on latest [Shibby build](http://tomato.groov.pl)


**Note: Use at your own risk. Be careful and thoughtful not to brick your router.**
