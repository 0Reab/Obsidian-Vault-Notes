Using these commands you can start, stop or see the status of the service running on your machine.
Those services could be ssh, mysql, apache2, etc...
You might need sudo before the command, depends on your configuration.

```shell
systemctl status $service   # status
service status $service

systemctl start $service    # start
service start $service

systemctl stop $service     # stop
service stop $service
```