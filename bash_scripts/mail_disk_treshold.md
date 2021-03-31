create a .sh file

```
#!/bin/bash
CURRENT=$(df / | grep / | awk '{ print $5}' | sed 's/%//g')
TEXT="Your root partition remaining free space is critically low. Used: $CURRENT%"

THRESHOLD=90

if [ "$CURRENT" -gt "$THRESHOLD" ] ; then
  echo $TEXT | mail -s 'Disk Space Alert' yourmail@mail.com
fi
```

then in your crontab set

```
@your_cron_frequency bash path_to_your_file.sh
```

restart cron process
