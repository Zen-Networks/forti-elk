![zen](index.png)

This project is about fortigate log monitoring with ELK stack (Elasticsearch, Logstash, Kibana). [Zen Networks](https://www.zen-networks.ma/)

I suppose that you are already configure Fortigate with your proper needs (filtering and users ...), and you install Elasticsearch, Logstash, Kibana, as well. In this project, you have to add some extra configuration like: 
- Fortigate configuration in order to send logs to specified port (in our case is 5517, you can do whatever you want).
- Logstash configuration.
Then all you have to do is importing the visualizations and the dashboard to your Kibana.


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
Elasticsearsh is listening to localhost:9200 in my case so if you separate the servers then you have to change localhost to Elasticsearsh address whit the relative port.

then start the logstash with this command:
```
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/fortigate.conf
```

# 3. Kibana Configurations:

- Go to Management then Saved Objects click on import in order to import Index, dashboard and visualizations that exit in Kibana folder.

- Make sur that the time is the same on both Fortigate and Kibana server.

