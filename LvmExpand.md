# Expansão de Disco LVM (sem reiniciar)

Guia rápido para expandir partição e sistema de arquivos após aumentar o disco da VM.

---

## 1. Verificar o disco

Veja se o SO já detectou o novo tamanho:

```
lsblk
sudo fdisk -l
```

2. Redimensionar a partição (se necessário)
Se o disco é /dev/sda e a partição do root é /dev/sda3 (exemplo), use o growpart:

````
sudo apt update && sudo apt install -y cloud-guest-utils
sudo growpart /dev/sda 3
````
🔹 Troque o número 3 pelo número da partição onde está o root ou LVM.

🔹 Se não aparecer force o rescan 
````
echo 1 | sudo tee /sys/class/block/nvme0n1/device/rescan
````

3. Expandir o sistema de arquivos
Agora depende de como está o layout do disco:

  a) Se for EXT4 direto na partição:
    
    sudo resize2fs /dev/sda3
  b) Se for XFS direto na partição:
    
    sudo xfs_growfs /

4. Se estiver usando LVM
Se a partição é um PV do LVM:

Expande o PV
````
sudo pvresize /dev/sda3
````
Descobre o nome do LV

````
sudo lvdisplay
````
Expande o LV (exemplo: ubuntu-vg/ubuntu-lv para usar todo o espaço)
````
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv

````
Ajusta o filesystem
EXT4
````
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
````
XFS
````
sudo xfs_growfs /
````
5. Conferir
Verifique se o espaço foi expandido com:
````
df -hT
````
