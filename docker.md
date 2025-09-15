Criando volume no nfs 

    docker volume create \
      --driver local \
      --opt type=nfs \
      --opt o=addr=192.168.0.100,nolock,rw \
      --opt device=:/nfs/shared \
      prometheus

Permissão de arquivo NFS 

    ls -ld /home/vagrant/app/prometheus
    sudo chown -R 65534:65534 prometheus/
    sudo chmod -R 775 prometheus/

Criando a rede overlay com attachable ( para containers fora do Swarm compartilharem a mesma rede)

    docker network create \
    --driver overlay \
    --attachable \
    my_overlay_network

Teste rede 

    netstat -tuln
    ss -tuln
