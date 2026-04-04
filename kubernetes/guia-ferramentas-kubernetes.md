# Guia de Ferramentas Kubernetes CLI

Ferramentas instaladas no bastion e WSL para facilitar o uso do Kubernetes no homelab.

---

## Ferramentas instaladas

| Ferramenta | Versão | Função |
|-----------|--------|--------|
| `kubectl` | v1.35.3 | CLI principal do Kubernetes |
| `kubecolor` | 0.5.3 | Output colorido automático |
| `kubectl-neat` | 2.0.4 | YAML limpo sem ruído |
| `stern` | 1.33.1 | Logs de múltiplos pods |
| `kubectx` | 0.11.0 | Troca de contexto |
| `kubens` | 0.11.0 | Troca de namespace |
| `kubectl-node-shell` | 1.11.0 | Shell root direto no node |

---

## kubecolor — kubectl colorido

Transparente, substitui o `kubectl` automaticamente via alias. Não muda o uso — só melhora a leitura.

```bash
kubectl get pods          # verde=Running, vermelho=Error
kubectl get nodes         # status dos nodes com cores
kubectl describe pod foo  # seções destacadas por cor
```

---

## kubectl-neat — YAML limpo

Remove o ruído gerado pelo Kubernetes (`managedFields`, `resourceVersion`, `uid`, timestamps).

**Sem neat** — YAML com 40+ linhas inúteis:
```yaml
metadata:
  managedFields:
    - apiVersion: apps/v1
      fieldsType: FieldsV1
      fieldsV1: ...
  resourceVersion: "123456"
  uid: a1b2c3d4-...
```

**Com neat:**
```bash
kubectl get pod foo -o yaml | neat       # via pipe
kneat get pod foo -o yaml               # wrapper direto
kneat get deployment nginx -o yaml > clean.yaml   # exportar limpo
```

> Ideal para copiar manifests de recursos existentes como base para novos.

---

## stern — logs de múltiplos pods

O `kubectl logs` só vê um pod por vez. O `stern` agrega todos com cores diferentes por pod.

```bash
# Todos os pods de um deployment
stern -n production api-gateway

# Por regex — qualquer pod com "worker" no nome
stern -n default worker

# Últimas 2h de logs
stern -n default api --since 2h

# Filtrar linhas que contêm ERROR ou WARN
stern -n default api --include "ERROR|WARN"

# Todos os pods de todos os namespaces
stern -A .

# Formato customizado — só a mensagem
stern -n default api --template '{{.Message}}'
```

---

## kubectx — troca de contexto

```bash
kubectx                   # lista contextos disponíveis
kubectx homelab-k8s       # muda para o cluster homelab
kubectx kind-girus        # muda para o kind local
kubectx -                 # volta para o contexto anterior (toggle)
```

> O p10k mostra o contexto atual no prompt automaticamente ao digitar comandos kubectl.

---

## kubens — troca de namespace

```bash
kubens                    # lista namespaces
kubens monitoring         # muda o namespace padrão
kubens default            # volta para default
```

Depois de `kubens monitoring`, todos os `kubectl get pods` já usam `monitoring` sem precisar de `-n monitoring`.

---

## kubectl-node-shell — shell root no node

Abre um pod privilegiado diretamente no node para inspecionar o host.

```bash
kubectl node-shell k8s-worker1    # shell root no worker1
kubectl node-shell k8s-master     # shell root no control-plane
```

Dentro do shell:
```bash
crictl ps                         # containers rodando via containerd
journalctl -u kubelet -f          # logs do kubelet em tempo real
cat /etc/kubernetes/manifests/    # manifests estáticos
```

---

## Combinações práticas

```bash
# Ver pods com problemas em todos os namespaces
kubectl get pods -A | grep -v Running | grep -v Completed

# Logs de todos os pods de um app filtrando erros
stern -n default meu-app --include "ERROR|WARN" --since 30m

# Exportar deployment limpo para editar e reaplicar
kneat get deployment nginx -o yaml > nginx.yaml
vim nginx.yaml
kubectl apply -f nginx.yaml

# Trocar contexto, namespace e inspecionar tudo
kubectx homelab-k8s
kubens monitoring
kubectl get all

# Debug direto no node onde o pod está rodando
NODE=$(kubectl get pod foo -o jsonpath='{.spec.nodeName}')
kubectl node-shell $NODE
```

---

## Aliases configurados (WSL e bastion)

| Alias | Comando completo |
|-------|-----------------|
| `k` | `kubectl` |
| `kgp` | `kubectl get pods` |
| `kgpa` | `kubectl get pods -A` |
| `kgn` | `kubectl get nodes` |
| `kgs` | `kubectl get svc` |
| `kgd` | `kubectl get deployments` |
| `kga` | `kubectl get all` |
| `kgaa` | `kubectl get all -A` |
| `kdp` | `kubectl describe pod` |
| `kl` | `kubectl logs` |
| `klf` | `kubectl logs -f` |
| `kex` | `kubectl exec -it` |
| `kns` | `kubectl config set-context --current --namespace` |
| `kctx` | `kubectl config use-context` |
| `kw` | `watch -n2 kubectl get pods` |
| `kwa` | `watch -n2 kubectl get pods -A` |
| `neat` | `kubectl-neat` |
| `kneat` | wrapper `kubectl ... \| kubectl-neat` |
