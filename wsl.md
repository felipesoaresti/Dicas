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

# Configurar Git com SSH no Windows e WSL

## 1. Gerar Chave SSH (Windows)

No PowerShell ou Git Bash:

```powershell
# Gerar nova chave SSH
ssh-keygen -t ed25519 -C "felipe.staypuff@gmail.com"

# Ou se preferir RSA
ssh-keygen -t rsa -b 4096 -C "felipe.staypuff@gmail.com"

# Salvar em: C:\Users\felip\.ssh\id_ed25519
```

## 2. Adicionar Chave SSH ao GitHub

```powershell
# Copiar chave pública para área de transferência
clip < C:\Users\felip\.ssh\id_ed25519.pub

# Ou exibir na tela
cat C:\Users\felip\.ssh\id_ed25519.pub
```

Depois:
1. Ir em https://github.com/settings/keys
2. Clicar em "New SSH key"
3. Cole a chave e salve

## 3. Configurar Git para usar SSH (Windows)

```powershell
# Verificar remote atual
git remote -v

# Mudar de HTTPS para SSH
git remote set-url origin git@github.com:felipesoaresti/Dicas.git

# Testar conexão
ssh -T git@github.com
```

## 4. Configurar SSH Agent (Windows)

```powershell
# Iniciar ssh-agent
Start-Service ssh-agent

# Configurar para iniciar automaticamente
Set-Service ssh-agent -StartupType Automatic

# Adicionar chave ao agent
ssh-add C:\Users\felip\.ssh\id_ed25519
```

## 5. Configurar Git no WSL

Dentro do WSL:

```bash
# Configurar usuário
git config --global user.name "Felipe Soares"
git config --global user.email "felipe.staypuff@gmail.com"

# Copiar chave SSH do Windows para WSL
mkdir -p ~/.ssh
cp /mnt/c/Users/felip/.ssh/id_ed25519 ~/.ssh/
cp /mnt/c/Users/felip/.ssh/id_ed25519.pub ~/.ssh/

# Ajustar permissões (importante!)
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 700 ~/.ssh

# Testar conexão
ssh -T git@github.com

# Configurar repositório para usar SSH
git remote set-url origin git@github.com:felipesoaresti/Dicas.git
```

## 6. Compartilhar Chaves SSH entre Windows e WSL (Recomendado)

Criar link simbólico no WSL para usar as chaves do Windows:

```bash
# No WSL
rm -rf ~/.ssh  # Remove pasta ssh do WSL se existir
ln -s /mnt/c/Users/felip/.ssh ~/.ssh

# Criar config SSH para ignorar permissões do Windows
cat > /mnt/c/Users/felip/.ssh/config << 'EOF'
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    StrictHostKeyChecking no
EOF
```

## 7. Configurar VSCode

### Source Control no Windows:
- VSCode vai usar o Git do Windows automaticamente
- Certifique-se que o remote está configurado para SSH

### Source Control no WSL:
1. Instalar extensão "WSL" no VSCode
2. Abrir pasta do WSL: `Ctrl+Shift+P` → "WSL: Open Folder in WSL"
3. Git do WSL será usado automaticamente

## 8. Testar Configuração

```powershell
# No Windows
git pull
git push

# No WSL
wsl
git pull
git push
```

## Solução de Problemas

### Erro de permissão SSH no WSL:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Mudar todos os repositórios de HTTPS para SSH:
```bash
# Windows (PowerShell)
git config --global url."git@github.com:".insteadOf "https://github.com/"

# WSL
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

