## Problema de baixar distros no vagrant 
    setx VAGRANT_SERVER_URL "https://app.vagrantup.com"
Feche e reabra o PowerShell, depois
    
    vagrant box add ubuntu/jammy64 --provider virtualbox
## Exemplo de um trecho do Vagrantfile, nao esquecer networks
NAT padrão (10.0.2.15) – não precisa declarar
Private/host-only (sua rede do cluster)
    
    master.vm.network "private_network", ip: "172.89.0.11"
Bridged (mesma LAN do Windows/WSL)
    
    master.vm.network "public_network", ip: "192.168.3.111"
  
