# zabbix-flume-template
Zabbix autodiscovery template and externalscript for monitoring Apache Flume via http.


### Installation
* Put check_flume_metrics.py to all Apache Flume hosts. By default directory is: /usr/lib/zabbix/externalscripts/. Change permissions to run this script:
```
$ chmod +x /usr/lib/zabbix/externalscripts/check_flume_metrics.py
```
* Put userparameter_flume.conf to /etc/zabbix/zabbix_agentd.d/userparameter_flume.conf (Modify this config, if you use different path for externascript)
* restart zabbix agent on Flume hosts.
* Import zabbix-flume-template to your Zabbix.
* If you change Apache Flume parameter -Dflume.monitoring.port (default 41414), then you need to edit monitoring port in Zabbix: Configuration -> Templates -> Template App Flume -> Macros -> {$FLUME_PORT}

### Test insallation


```
usage: check_flume_metrics.py [-h] [--discover type] [--check FLOW METRIC]
                              [--diff FLOW METRIC1 METRIC2]
                              [--address ADDRESS] [--port PORT]

Simple python script for checking flume metrcis via http

optional arguments:
  -h, --help            show this help message and exit
  --discover type, -d type
                        Discover all flume flows by type
  --check FLOW METRIC, -c FLOW METRIC
                        Check specified metric
  --diff FLOW METRIC1 METRIC2, -f FLOW METRIC1 METRIC2
                        Check difference between two specified metrics
  --address ADDRESS, -a ADDRESS
                        Flume address. Default is: localhost
  --port PORT, -p PORT  Flume http port. Default is: 41414
```

If you installed zabbix_get package, you may run :
```
zabbix_get -s flumehost1 -k flume.check[41414,"CHANNEL.prod-channel","ChannelFillPercentage"]

```

#### Examples

Discovery:

```
$ sudo -u zabbix /etc/zabbix/externalscripts/check_flume_metrics.py --discover SOURCE
{"data": [{"{#NAME}": "SOURCE.test-source"}, {"{#NAME}": "SOURCE.prod-source"}, {"{#NAME}": "SOURCE.third-source"}]}

```

Check:

```
$ sudo -u zabbix /etc/zabbix/externalscripts/check_flume_metrics.py --check SOURCE.test-source KafkaEventGetTimer
28030074
```

#### Notes
* Tested with Zabbix 3.2 and Flume CDH-5.7.0-1.cdh5.7.0.p0.45 version.