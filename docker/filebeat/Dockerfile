FROM airdock/oracle-jdk
MAINTAINER wangzhu wangzhu@originaltek.com
COPY filebeat-6.5.1-amd64.deb /root
RUN  ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
     && echo "Asia/Shanghai" > /etc/timezone && \
     dpkg -i  /root/filebeat-6.5.1-amd64.deb && \
     rm -f /root/filebeat-6.5.1-amd64.deb
CMD /usr/share/filebeat/bin/filebeat -e -c /etc/filebeat/filebeat.yml



