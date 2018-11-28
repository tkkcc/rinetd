```sh
# iptables wget systemctl
killall -9 rinetd
url="https://raw.githubusercontent.com/tkkcc/rinetd/master/rinetd"
wget "$url" -O /usr/bin/rinetd
chmod +x /usr/bin/rinetd
echo '0.0.0.0 40000 0.0.0.0 40000' > /etc/rinetd.conf
iface=$(ip -4 addr | awk '{if ($1 ~ /inet/ && $NF ~ /^[ve]/) {a=$NF}} END{print a}')
cat>/etc/systemd/system/rinetd.service <<EOF
[Service]
ExecStart=/usr/bin/rinetd -f -c /etc/rinetd.conf raw $iface
ExecStop=/usr/bin/killall -9 rinetd
[Install]
WantedBy=multi-user.target
EOF
systemctl restart rinetd
systemctl status -l --full --no-pager rinetd
systemctl enable rinetd
```
> same as [nanqinlang](https://github.com/tcp-nanqinlang/lkl-rinetd), better than [linhua55](https://github.com/linhua55/lkl_study) in my case
