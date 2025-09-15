GIT - SSH / Dicas e procediementos 

    $ssh-keygen -t ed25519 -C "your_email@example.com"
    $ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    $eval $(ssh-agent)

Saídas: $ssh-agent

    $ SSH_AUTH_SOCK=/tmp/ssh-XXXXXX/agent.1234; export SSH_AUTH_SOCK;  
    $ SSH_AGENT_PID=1234; export SSH_AGENT_PID;

    $ ssh-add /caminho/para/sua_chave
    $ ssh-add -l #listar a chave
    $ ssh -T git@github.com  # Testar git hub

Clonar ou usar um repositório
Agora, você pode clonar repositórios usando SSH. Exemplo:

    $ git clone git@github.com:username/repo.git

Substitua username e repo pelos respectivos valores.

    $ git clone git@github.com:username/meu-projeto.git
    $ echo "# Meu Projeto" > README.md

Enviar mudanças para o GitHub
Para salvar mudanças no GitHub, use os comandos abaixo:
Adicionar os arquivos ao índice do Git:

    $ git add .

Criar um commit:

    $ git commit -m "Primeiro commit"

Enviar para o GitHub:

    $ git push origin main

Confirmar no GitHub
Verificar o status dos arquivos:

    $ git status

Adicionar e enviar novas mudanças:

Acesse o diretório local
Entre no diretório /app onde estão os arquivos que deseja enviar:

     $ cd /app
Inicialize o Git no diretório local
Se o diretório ainda não estiver versionado, inicialize um repositório Git:

    $ git init

Conecte ao repositório remoto no GitHub
Adicione o repositório remoto como destino:

     $git remote add origin git@github.com:yourusername/app.git

Para verificar se o remoto foi adicionado corretamente:

    $ git remote -v

Você deve ver algo assim:

    origin  git@github.com:yourusername/app.git (fetch)
    origin  git@github.com:yourusername/app.git (push)

Adicionar os arquivos ao repositório local
Adicione todos os arquivos do diretório /app ao índice do Git:

    $ git add .

Criar o primeiro commit
Crie um commit com uma mensagem descritiva:

    $ git commit -m "Primeiro commit: adicionando arquivos do diretório local"

Enviar os arquivos para o GitHub
Envie os arquivos para o repositório remoto na branch main (ou master, dependendo do nome padrão do repositório):
    
    $ git branch -M main  # Caso ainda não tenha a branch main
    $ git push -u origin main

Verificar no GitHub
Acesse o repositório no GitHub (https://github.com/yourusername/app) para confirmar que os arquivos foram enviados corretamente.

Observação:
 - O comando git branch -M main define o nome da branch principal como main. Se o repositório remoto usar master, altere o comando de push para:

       $ git push -u origin master
       $ git add .
       $ git commit -m "Descrição da mudança"
       $ git push origin main

Configurar o nome e o e-mail no Git
Execute os seguintes comandos:

    $ git config --global user.name "Seu Nome"
    $git config --global user.email "felipe.staypuff@gmail.com"

O parâmetro --global aplica essas configurações para todos os repositórios no seu sistema.
Caso queira configurar apenas para este repositório, omita o --global:

    git config user.name "Seu Nome"
    git config user.email "felipe.staypuff@gmail.com"

Verificar as configurações
Depois de configurar, você pode verificar se os valores foram definidos corretamente:

    $ git config --list

Você verá algo como:

    user.name=Seu Nome
    user.email=felipe.staypuff@gmail.com

Refaça o commit
Agora que o Git está configurado, você pode refazer o commit normalmente:

     $ git commit -m "App´s rodando no Docker Swarm"

Erro quando o repositório remoto no GitHub contém commits (provavelmente uma inicialização vazia) que não estão presentes no seu repositório local. O Git não permite o envio para o remoto quando o repositório está desatualizado em relação ao repositório remoto.

Solução:

Atualize seu repositório local com as alterações do repositório remoto
Primeiro, é necessário baixar os dados do repositório remoto (sem fazer merge) para poder integrar as alterações.

Execute o seguinte comando para fazer um git pull do repositório remoto:

    $ git pull origin main --allow-unrelated-histories

--allow-unrelated-histories: Permite combinar históricos de repositórios diferentes (no seu caso, o repositório local sem commits e o repositório remoto vazio).

Resolver possíveis conflitos
Se houver conflitos, o Git pedirá para resolvê-los. Use um editor para editar os arquivos conflitantes, e depois marque-os como resolvidos:

    $ git add arquivo-conflito
    $ git commit -m "Resolvendo conflitos"

Fazer o push novamente
Depois de integrar as alterações, tente enviar suas mudanças para o GitHub novamente:

    $ git push -u origin main
    $ git push -u origin main --force

