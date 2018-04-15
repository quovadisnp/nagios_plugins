# nagios_plugins
Miscellaneous collection of custom Nagios plugins

## The Plugins

### check_temp
Check and return temperature status. Currently only support CPU temp check. Requires the lm_sensors package.

#### Option -c
Output
```
CPU Avg @ 37.25C
Core2 OK @ 34.000C
Core3 OK @ 33.000C
Core0 OK @ 38.000C
Core1 OK @ 44.000C
```
Returns an exit code for Nagios of OK, Warning, or Critical based on the what lm_sensors states the thresholds as.

## Usage

### On each client place script in your Nagios plugins folder (your folder could very well be different than mine). Run the plugin directly to make sure it works without any issues or missing dependencies (e.g. ./check_temp -c)
```
/usr/local/nagios/libexec/
```

### On each client edit nrpe.cfg to create a command to call the desired plugin
```
vi /etc/nagios/nrpe.cfg
```
#### check_temp (cpu) option
```
command[check_cpu]=/usr/local/nagios/libexec/check_temp -c
```

### Add service check to server for each host to reference the new client command
```
vi /usr/local/nagios/etc/services.cfg
```

#### check_temp (cpu) option
```
define service {
        use                     generic-service
        host_name               myhost
        service_description     CPU Temperature
        check_command           check_cpu
        servicegroups           health
}
```
