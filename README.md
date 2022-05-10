# check_linux_multipath
nagios check for linux multipath SAN disk path status

# Requirements
perl, SSH key pair auth

# Configuration

Add a section similar to the following to the services.cfg file on the nagios server.
```
define service{
        use                             generic-24x7-service
        host_name                       linux01.example.com
        service_description             dm-multipath
        check_command                   check_by_ssh!"/usr/local/nagios/libexec/check_linux_multipath"
        }
```

Please note that this script executes the /usr/sbin/multipathd command, which requires root privileges on most Linux distributions.
To avoid giving the low-privileged nagios user permission to run multipathd via sudo, this script is run from the root crontab, which creates a temporary file in /tmp/ that is then read when the low-privilege nagios user runs the check.  Please create an entry in the root crontab similar to the following:
```
1,6,11,16,21,26,31,36,41,46,51,56 * * * * /usr/local/nagios/libexec/check_linux_multipath  1>/dev/null 2>/dev/null #nagios helper script
```

# Output
You will see output similar to the following:
```
<font color=green>dm-multipath SAN disk paths OK - active:80 passive:0 faulty:0 shaky:0 </font>
<font color=red>dm-multipath SAN disk paths WARN - active:40 passive:0 faulty:40 shaky:0 </font>
```

