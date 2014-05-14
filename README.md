v4.06-9437-beta

You will need to setup the init script and also chkconfig level

This is my version with dnsmasq

# cat /etc/init.d/vpnserver 
#!/bin/sh
# Provides:          vpnserver
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable Softether by daemon.
### END INIT INFO
DAEMON=/usr/local/vpnserver/vpnserver
LOCK=/var/lock/subsys/vpnserver
TAP_ADDR=10.1.1.1

test -x $DAEMON || exit 0
case "$1" in
start)
echo "re-starting dnsmasq"
/etc/init.d/dnsmasq stop
sleep 3
/etc/init.d/dnsmasq start
sleep 3
$DAEMON start
touch $LOCK
sleep 1
/sbin/ifconfig tap_soft $TAP_ADDR
;;
stop)
$DAEMON stop
rm $LOCK
;;
restart)
$DAEMON stop
sleep 3
$DAEMON start
sleep 1
/sbin/ifconfig tap_soft $TAP_ADDR
;;
*)
echo "Usage: $0 {start|stop|restart}"
exit 1
esac
exit 0
