# 📚 Dicas & Anotações Técnicas

Repositório de anotações, guias e referências rápidas sobre Linux, Kubernetes, Docker, Proxmox e ferramentas de infraestrutura.

---

## 📂 Estrutura

```
.
├── kubernetes/     → kubectl, kubeconfig, ferramentas CLI
├── linux/          → LVM, WSL, DNS, portas, scripts do sistema
├── docker/         → Docker, ctop, Vagrant, VirtualBox
├── ferramentas/    → curl, Git SSH Agent
├── proxmox/        → NFS, oVirt, configurações de cluster
└── checkmk/        → Plugins e configurações de monitoramento
```

---

## ☸️ Kubernetes

| Arquivo | Descrição |
|---------|-----------|
| [guia-ferramentas-kubernetes.md](kubernetes/guia-ferramentas-kubernetes.md) | Guia completo das ferramentas CLI: kubecolor, kubectl-neat, stern, kubectx, kubens, node-shell |
| [kubeconfig-merge.md](kubernetes/kubeconfig-merge.md) | Como mesclar múltiplos kubeconfigs (Windows e Linux) |

---

## 🐧 Linux

| Arquivo | Descrição |
|---------|-----------|
| [lvm-expand.md](linux/lvm-expand.md) | Expansão de volume LVM |
| [lvm-expand-oracle9.md](linux/lvm-expand-oracle9.md) | Expansão de LVM no Oracle Linux 9 |
| [portas-abertas.md](linux/portas-abertas.md) | Como verificar portas abertas no sistema |
| [wsl.md](linux/wsl.md) | Dicas e configurações do WSL (Windows Subsystem for Linux) |
| [dns-flush-cache.md](linux/dns-flush-cache.md) | Como limpar o cache DNS em diferentes sistemas |

---

## 🐳 Docker & Virtualização

| Arquivo | Descrição |
|---------|-----------|
| [docker.md](docker/docker.md) | Comandos e dicas essenciais do Docker |
| [ctop.md](docker/ctop.md) | Monitoramento de containers com ctop |
| [vagrant.md](docker/vagrant.md) | Uso e configuração do Vagrant |
| [virtualbox.md](docker/virtualbox.md) | Dicas de configuração do VirtualBox |

---

## 🔧 Ferramentas

| Arquivo | Descrição |
|---------|-----------|
| [curl.md](ferramentas/curl.md) | Uso avançado do curl |
| [git-ssh-agent.md](ferramentas/git-ssh-agent.md) | Configuração do SSH Agent para Git no WSL |

---

## 🖥️ Proxmox

| Arquivo | Descrição |
|---------|-----------|
| [nfs-ovirt.md](proxmox/nfs-ovirt.md) | Configuração de NFS para oVirt/Proxmox |

---

## 📊 Checkmk

| Arquivo | Descrição |
|---------|-----------|
| [plugin-curl.md](checkmk/plugin-curl.md) | Plugin curl para monitoramento no Checkmk |

---

## 🔗 Repositórios relacionados

| Repo | Descrição |
|------|-----------|
| [Cloudflare-Metallb-NginxIngress-Controler](https://github.com/felipesoaresti/Cloudflare-Metallb-NginxIngress-Controler) | Expor homelab atrás de CGNAT com Cloudflare Tunnel + MetalLB + NGINX |
| [Rancher](https://github.com/felipesoaresti/Rancher) | Cluster RKE2 + Rancher em HA no homelab |
| [Firewall](https://github.com/felipesoaresti/Firewall) | Scripts iptables para Debian |
| [scripts](https://github.com/felipesoaresti/scripts) | Shell scripts utilitários |
| [cmdbsyncer-plugins](https://github.com/felipesoaresti/cmdbsyncer-plugins) | Plugins para CMDBSyncer (Checkmk, Netbox) |
