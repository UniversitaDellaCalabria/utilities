```sudo apt-get install bc```

create a .sh file

```
#!/bin/sh
ramusage=$(free | awk '/Mem/{printf("RAM Usage: %.2f\n"), $3/$2*100}'| awk '{print $3}')

if [ $(echo "$ramusage>80"| bc) -eq 1 ] ; then
SUBJECT="ATTENTION: Memory Utilization is High on $(hostname) at $(date)"
MESSAGE="/tmp/Mail.out"
TO="yourmail@mail.com"

  echo "Memory Current Usage is: $ramusage%" >> $MESSAGE
  echo "" >> $MESSAGE
  echo "------------------------------------------------------------------" >> $MESSAGE
  echo "Top Memory Consuming Process Using top command" >> $MESSAGE
  echo "------------------------------------------------------------------" >> $MESSAGE
  echo "$(top -b -o +%MEM | head -n 20)" >> $MESSAGE
  echo "" >> $MESSAGE
  echo "------------------------------------------------------------------" >> $MESSAGE
  echo "Top Memory Consuming Process Using ps command" >> $MESSAGE
  echo "------------------------------------------------------------------" >> $MESSAGE
  echo "$(ps -eo pid,ppid,%mem,%cpu,cmd --sort=-%mem | head)" >> $MESSAGE
  mail -s "$SUBJECT" "$TO" < $MESSAGE
  rm /tmp/Mail.out
  fi
```

then in your crontab set

```
@your_cron_frequency bash path_to_your_file.sh
```

restart cron process
