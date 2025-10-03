# Comandos no PowerShell / Prompt (fora do WSL)

## Esses são usados para gerenciar as distribuições Linux no WSL:

Ver versão do WSL instalada

	wsl --version

Listar distribuições instaladas

	wsl --list --verbose
	wsl -l -v

Definir distribuição padrão

	wsl --set-default <NomeDaDistro>

Definir versão do WSL (1 ou 2) para uma distro

	wsl --set-version <NomeDaDistro> 2

	Iniciar o WSL (entra na distro padrão)

	wsl

Iniciar uma distro específica

	wsl -d Ubuntu

Parar todas as distros em execução

	wsl --shutdown


## Integração Windows ↔ Linux

Acessar arquivos do Windows dentro do WSL:
Estão em /mnt/c/Users/SeuUsuario/

Rodar comando Linux direto do Windows (PowerShell):

	wsl ls -la

## Gerenciar múltiplas distros no WSL

Instalar uma nova distribuição
Você pode instalar várias distros lado a lado.

	wsl --install -d <NomeDaDistro>
	wsl --install -d Ubuntu-22.04
	wsl --install -d Debian
	wsl --install -d openSUSE-Leap-15.5

Listar todas as distros instaladas

	wsl -l -v

Definir uma distro padrão

	wsl --set-default <NomeDaDistro>

Rodar uma distro específica

	wsl -d <NomeDaDistro>

Desinstalar uma distro
Esse comando remove a distro e todos os dados dela (como se fosse um "reset"):

	wsl --unregister <NomeDaDistro>

Instalar a mesma distro mais de uma vez
Se você quiser duas instâncias de Ubuntu, por exemplo:

Exportar a distro já instalada:

wsl --export Ubuntu-22.04 C:\backup\ubuntu.tar

Importar como uma nova distro com outro nome:

wsl --import Ubuntu-Test C:\WSL\UbuntuTest C:\backup\ubuntu.tar --version 2

Isso mostra todas as distros que você pode instalar.

	wsl -l -o

Instalar uma distro disponível
Depois que ver a lista, basta instalar, por exemplo:

	wsl --install -d Ubuntu-22.04

