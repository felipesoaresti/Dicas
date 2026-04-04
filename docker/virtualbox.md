Habilitar VTX /AMD-V no VirtualBoX 

Windows:
Use estes comandos:

cd C:\Program Files\Oracle\VirtualBox

VBoxManage.exe list vms

VBoxManage.exe modifyvm "GNS3 VM" --nested-hw-virt on

______________________

Desative o Hyper-V e o Hypervisor do Windows 11
Abra o Prompt de Comando ou PowerShell como Administrador e execute:

bcdedit /set hypervisorlaunchtype off

Depois, reinicie o computador.

Esse comando desativa o Hypervisor do Windows que entra em conflito com o VMware e o VirtualBox.

2. Desative recursos conflitantes no Windows
Vá em:

Painel de Controle > Programas e Recursos > Ativar ou desativar recursos do Windows

Desmarque tudo isso:

- Hyper-V
- Plataforma de Máquina Virtual
- Windows Hypervisor Platform
- Plataforma de Computação Segura (se aparecer)

Clique em OK, e reinicie novamente.

3. Desative a "Integridade da Memória" (Memory Integrity)
Vá em Configurações > Privacidade e Segurança > Segurança do Windows > Segurança do Dispositivo

Clique em Isolamento do Núcleo

Desative "Integridade da Memória"

Reinicie mais uma vez
