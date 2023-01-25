docker run -it -d --name ai4healthsecsnort --mount type=bind,source=$PWD/rules,target=/usr/local/etc/snort/rules --mount type=bind,source=$PWD/logs,target=/var/log/snort --net=host satchm0h/alpine-snort3

#create rules on /rules folder outside the container
snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/snort/rules/snort3.rules -i ens18 -l /var/log/snort/ -i ens18 -m 0x1b &

alert_json =
{
    file = true, -- write the output to a file
    limit = 100, -- Create a new file after every 100M
    fields = 'seconds action class b64_data dir dst_addr dst_ap dst_port eth_dst eth_len \
    eth_src eth_type gid icmp_code icmp_id icmp_seq icmp_type iface ip_id ip_len msg mpls \
    pkt_gen pkt_len pkt_num priority proto rev rule service sid src_addr src_ap src_port \
    target tcp_ack tcp_flags tcp_len tcp_seq tcp_win tos ttl udp_len vlan timestamp',
}



#[evt.security]
#LogName= c:\log.txt
#Index="bloodraven_win"
#Rules=["outputs.metago"]

## Metrics input
# Collects system metrics from the local machine
# CPU,DISK,NET,MEMORY,Connections and Process information
[metrics.all]
Metrics=[
    "cpu", # Cpu time consumed
    "cpuinfo", # Cpu version, and spec
    "virtualmem", # Memory used/free
    "disk", # Disk IO counters
    "loadavg", # CPU load average 1,5,15m
    "net", # Net IO Counters by interface
    "connections",# Connection,src,dst,ports,process pid, type, status
    "process"]
Index= "bloodraven_metrics"
Rules=["outputs.metago"]

[outputs.metago]
Urls = [ "metagos://ai4health.pdmfc.com/metadon/dynamic-ports/10000/:80/" ]
#Urls = [ "metagos://siem.preprod.pdmfc.com/dynamic-ports/10000/:80/" ]
