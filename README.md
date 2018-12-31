![zen](index.png)

This project builds a Fortigate log monitoring solution based on ELK stack (Elasticsearch, Logstash, Kibana) and Fortigate firewalls logs. Courtesy of [Zen Networks](https://www.zen-networks.ma/)

# 0. Prerequisites and Scope:
- One or multiple Fortigate firewalls configured with the required filtering and identification features.
- Installed Elasticsearch, Logstash and Kibana instances.

In this project, we will cover: 
- Fortigate configuration in order to send logs to a specified host/port. We've choosen port 5517. But, it can be any valid port.
- Logstash configuration to parse Fortigate logs
- Kibana visualizations and dashboard to leverage these logs

# 1. Fortigate Configurations:
```
FortiGate-VM-1 # config log syslogd setting 
FortiGate-VM-1 (setting) # show full-configuration 
config log syslogd setting
    set status enable
    set server "192.168.1.70"
    set mode udp
    set port 5517 
    set facility local7
    set source-ip ''
    set format default
end
```

```
FortiGate-VM-1 # config log setting  
FortiGate-VM-1 (setting) # show full-configuration 
config log setting
    set resolve-ip disable
    set resolve-port enable
    set log-user-in-upper disable
    set fwpolicy-implicit-log enable
    set fwpolicy6-implicit-log disable
    set log-invalid-packet disable
    set local-in-allow enable
    set local-in-deny-unicast enable
    set local-in-deny-broadcast enable
    set local-out enable
    set daemon-log disable
    set neighbor-event disable
    set brief-traffic-format disable
    set user-anonymize disable
    set expolicy-implicit-log disable
    set log-policy-comment disable
    set log-policy-name enable
end
```

# 2. Logstash Configurations:

Copy the file _fortigate.conf_ that exist in the folder Logstash to this path:
```
/etc/logstash/conf.d/
```
Elasticsearch is listening to localhost:9200 in our case. If you have separated the ELK stack in a larger setup, then you have to change localhost to the Elasticsearch server address.

Start the logstash with this command:
```
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/fortigate.conf
```

# 3. Kibana Configurations:

- Go to Management. Then, Saved Objects. Click on import in order to import Index, dashboard and visualizations that exit in Kibana folder.

- Make sure that the time is the same on both Fortigate and Kibana server.

