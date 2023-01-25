docker run -it -d --name ai4healthsecsnort --mount type=bind,source=$PWD/rules,target=/usr/local/etc/snort/rules --mount type=bind,source=$PWD/logs,target=/var/log/snort --net=host satchm0h/alpine-snort3

#create rules on /rules folder outside the container
snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/snort/rules/snort3.rules -i ens18 -l /var/log/snort/ -i ens18 -m 0x1b &
