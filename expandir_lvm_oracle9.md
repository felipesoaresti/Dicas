# Expansão de Disco e LVM no Oracle Linux 9.5

Este guia mostra como aumentar o disco de uma VM e expandir o LVM em um sistema **Oracle Linux 9.5**.

---

## 1. Verificar os discos
Liste os discos e partições disponíveis:
```bash
lsblk
fdisk -l
```

---

## 2. Expandir a partição
Caso o LVM esteja dentro de uma partição (ex.: `/dev/sda3`), use o **growpart**:

```bash
sudo dnf install cloud-utils-growpart -y
sudo growpart /dev/sda 3
```

Verifique novamente:
```bash
lsblk
```

---

## 3. Expandir o PV (Physical Volume)
Redimensione o PV do LVM:
```bash
sudo pvresize /dev/sda3
```
Confirme:
```bash
pvs
```

---

## 4. Expandir o LV (Logical Volume)
Liste os volumes:
```bash
lvdisplay
```

Aumente o LV desejado (exemplo: root):
```bash
sudo lvextend -r -l +100%FREE /dev/mapper/ol-root
```

Explicação:
- `-r` → executa o `resize2fs` ou `xfs_growfs` automaticamente.
- `-l +100%FREE` → usa todo o espaço livre.

---

## 5. Conferir
```bash
df -h
lsblk
```

---


