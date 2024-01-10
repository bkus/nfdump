# nfdump NetFlow collector setup on Fedora 39

    $ sudo su -
    # dnf -y install nfdump
    # mkdir /var/log/netflow
    # chown nobody:nobody /var/log/netflow
    # firewall-cmd --permanent --zone=public --add-port=2055/udp
    # firewall-cmd --reload
    # cp netflow /etc/sysconfig/
    # cp netflow.service /etc/systemd/system/
    # systemctl daemon-reload
    # systemctl enable --now netflow
    
Go and setup a netflow source on a router and point it at your new service.  Once data arrives, there'll be a new directory in /var/log/netflow.
    
    # nfexpire -u /var/log/netflow/* -t 8w
    # exit
    $ echo 'alias nfd="nfdump -r /var/log/[your favorite netflow source dir]"' >> ~/.bashrc
    $ . ~/.bashrc
    $ nfd | less
