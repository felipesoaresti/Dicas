GIT - SSH / Dicas e procediementos 

ssh-keygen -t ed25519 -C "your_email@example.com"

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

eval $(ssh-agent)

> Agent pid 59566

#saídas: $ssh-agent

SSH_AUTH_SOCK=/tmp/ssh-XXXXXX/agent.1234; export SSH_AUTH_SOCK;
SSH_AGENT_PID=1234; export SSH_AGENT_PID;

ssh-add /caminho/para/sua_chave

ssh-add -l #listar a chave

ssh -T git@github.com  # Testar git hub

Clonar ou usar um repositório
Agora, você pode clonar repositórios usando SSH. Exemplo:


git clone git@github.com:username/repo.git
Substitua username e repo pelos respectivos valores.


git clone git@github.com:username/meu-projeto.git


echo "# Meu Projeto" > README.md

Enviar mudanças para o GitHub
Para salvar mudanças no GitHub, use os comandos abaixo:

Adicionar os arquivos ao índice do Git:

git add .

Criar um commit:

git commit -m "Primeiro commit"

Enviar para o GitHub:

git push origin main

Confirmar no GitHub

Verificar o status dos arquivos:

git status

Adicionar e enviar novas mudanças:

git add .

git commit -m "Descrição da mudança"

git push origin main

Atualizar o repositório local com mudanças do GitHub:

git pull origin main

Pronto! Seu repositório está criado e configurado. 🚀
