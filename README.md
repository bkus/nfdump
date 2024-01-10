# nfdump collector setup on Fedora 39
    $ sudo su -
    # dnf -y install nfdump
    # firewall-cmd --permanent --zone=public --add-port=2055/udp
    # firewall-cmd --reload
    # echo 'NFCAPD_OPTIONS="-S 2 -M /var/log/netflow -p 2055 -z=lz4 -e"' > /etc/sysconfig/netflow
    # mkdir /var/log/netflow
    # chown nobody:nobody /var/log/netflow
    # echo '[Unit]
    Description=NetFlow Logging Service
    Documentation=man:nfcapd(1)
    Documentation=https://github.com/phaag/nfdump
    
    [Service]
    EnvironmentFile=-/etc/sysconfig/netflow
    User=nobody
    Group=nobody
    ExecStart=/usr/bin/nfcapd $NFCAPD_OPTIONS
    Restart=on-failure
    KillSignal=SIGINT
    
    [Install]
    WantedBy=multi-user.target' > /etc/systemd/system/netflow.service
    # systemctl daemon-reload
    # systemctl enable --now netflow
    
    ## Go and setup a netflow source on a router and point it at your new server
    ## Once data arrives, there'll be a new directory in /var/log/netflow
    
    # nfexpire -u /var/log/netflow/* -t 8w
    # exit
    $ echo 'alias nfd="nfdump -r /var/log/[your favorite netflow source dir]' >> ~/.bashrc
    $ . ~/.bashrc
    $ nfd | less
