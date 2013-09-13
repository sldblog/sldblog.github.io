---
categories: sldcode
layout: post
title: JMX setup, how *not* to do it
---

You may want to monitor your Java server with `jconsole`. It once happened to us while using a custom tomcat installation that it simply failed to finish the deploy scripts. The problem was that the JMX options were included in `catalina.sh`:

```bash
JAVA_OPTS="$JAVA_OPTS -Djava.net.preferIPv4Stack=true"
JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.port=$JMXPORT"
JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.authenticate=false"
JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.ssl=false"
JAVA_OPTS="$JAVA_OPTS -Djava.rmi.server.hostname=$SERVERIP"
```

When the shutdown script tried to run, it thrown a somewhat weird exception:

```
Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: <port> nested exception is:
    java.net.BindException: Address already in use
```

It turns out that `shutdown.sh` invokes `catalina.sh` directly, by passing "stop" and including all other parameters:

```bash
EXECUTABLE=catalina.sh
[...]
exec "$PRGDIR"/"$EXECUTABLE" stop "$@"
```

Moving the options to `startup.sh` wouldn't have been trivial, so we added a simple if to keep this code from running when the server is shutting down:

```bash
if [ "$1" = "start" ]; then
  JAVA_OPTS="$JAVA_OPTS -Djava.net.preferIPv4Stack=true"
  JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.port=$JMXPORT"
  JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.authenticate=false"
  JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote.ssl=false"
  JAVA_OPTS="$JAVA_OPTS -Djava.rmi.server.hostname=$SERVERIP"
fi
```
