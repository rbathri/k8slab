##Capture all the events from the live system and print them to screen
```
  $ sysdig
```

##Capture all the events from the live system and save them to disk
```
$ sysdig -w cloudyuga-net.scap

```

##Capture all the events in the latest 24 hours and save them to disk organized in files containing 1 hour of system activity each

```
$ sysdig -G 3600 -W 3 -w cloudyuga-net-2.scap

```

## Read events from a file and print them to screen

```
$ sysdig -r cloudyuga-net-2.scap

```

##Prepare a sanitized version of a system capture

```
$ sysdig -r cloudyuga-net-3.scap 'not evt.buffer contains foo' -w sanitized.scap

```

##Print all the open system calls invoked by cat

```
$ sysdig proc.name=cat and evt.type=open

```

##Print the name of the files opened by cat

```
$ sysdig -p"%evt.arg.name" proc.name=cat and evt.type=open

```

##List the available chisels

```
$ sysdig -cl

```

##Use the spy_ip chisel to look at the data exchanged with 192.168.1.157:

```
$ sysdig -c spy_ip `curl ifconfig.me`

```

##Watch out all the commands in action!!

