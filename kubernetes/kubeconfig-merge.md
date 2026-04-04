## Dicas Kubectl
# Varios configs de Cluster diferentes no mesmo arquivo config. 
Obs: Antes do merge verificar usuários nos configs 

# No Windows:

Execute no CMD ou crie uma bat

````
#REM (opcional) backup
copy "%USERPROFILE%\.kube\config" "%USERPROFILE%\.kube\config.bak"

#REM 1) Set temporário com separador ;
set KUBECONFIG=%USERPROFILE%\.kube\cluster1.yaml;%USERPROFILE%\.kube\cluster2.yaml;%USERPROFILE%\.kube\cluster3.yaml

#REM 2) Mescla e grava no config padrão
kubectl config view --merge --flatten > "%USERPROFILE%\.kube\config"

#REM 3) (opcional) limpar
set KUBECONFIG=

````
# No LINUX

Execute na linha de comando ou crie um executavél 
( Não esquecer da permissão +x chmod +x)

````
#!/bin/bash

# 0) Backup opcional do config atual
cp -f ~/.kube/config ~/.kube/config.bak 2>/dev/null

# 1) Liste os arquivos que quer mesclar
FILES="$HOME/.kube/cluster1.yaml:$HOME/.kube/cluster2.yaml:$HOME/.kube/cluster3.yaml"

# 2) Exporte a variável KUBECONFIG com separador ":"
export KUBECONFIG=$FILES

# 3) Gere um único arquivo achatado (merge + flatten)
kubectl config view --merge --flatten > ~/.kube/config

# 4) Limpe a variável
unset KUBECONFIG

````

# Dicas úteis pós-merge

Listar contextos
````
kubectl config get-contexts
`````
Mudar o contexto ativo
````
kubectl config use-context nome-do-contexto
````
Renomear contexto (para ficar fácil de lembrar)
````
kubectl config rename-context contexto-antigo contexto-novo
````
Definir namespace padrão de um contexto
````
kubectl config set-context contexto-novo --namespace=meu-namespace
````
Verificar cluster ativo
````
kubectl cluster-info
kubectl get nodes
````
Se houver nomes iguais (users/clusters/contexts) entre arquivos
Renomeie antes de mesclar para evitar conflito:
````
kubectl config rename-context old@cluster1 cluster1
kubectl config rename-context old@cluster2 cluster2
````
