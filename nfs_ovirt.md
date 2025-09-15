  Criar o compartilhamento nfs

    systemctl daemon-reload
 
    systemctl enable rpcbind.service
    
    systemctl enable nfs-server.service
    
    yum install nfs-utils
    
    systemctl start rpcbind.service
    
    systemctl start nfs-server.service

    vim /etc/exports

/exports/data *(rw)
/exports/export *(rw)

    exportfs -r

    systemctl reload nfs-server.service

    groupadd kvm -g 36
    useradd vdsm -u 36 -g 36

    chown -R 36:36 /exports/*

    chmod 0755 /exports/*

Converter o disco vmdk qcow2

    qemu-img convert -cpf vmdk focus01-0.vmdk -O qcow2 -o preallocation=off  focus01-0.qcow2
Firewall-cmd para o compartilhamento NFS 

    firewall-cmd --permanent --add-service=nfs
    firewall-cmd --permanent --add-service=mountd
    firewall-cmd --permanent --add-service=rpc-bind
    firewall-cmd --reload
