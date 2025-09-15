# How to Flush the DNS Cache in Linux?

## Table of Contents
- [What is a DNS Cache?](#what-is-a-dns-cache)
- [Find Your Local DNS Resolver](#find-your-local-dns-resolver)
- [How to Flush the DNS Cache on Linux?](#how-to-flush-the-dns-cache-on-linux)
  - [Flush DNS Cache Using `systemd-resolved`](#flush-dns-cache-using-systemd-resolved)
  - [Clear DNS Cache Using Signals](#clear-dns-cache-using-signals)
  - [Flush DNS Cache Using `dnsmasq`](#flush-dns-cache-using-dnsmasq)
  - [Clear DNS Cache Using `nscd` in RedHat](#clear-dns-cache-using-nscd-in-redhat)
- [Clear DNS Cache on Google Chrome](#clear-dns-cache-on-google-chrome)

---

## What is a DNS Cache?

O **Domain Name System (DNS)** é um sistema global que mapeia nomes de domínio para seus endereços IP correspondentes. O cache DNS é um banco de dados temporário armazenado pelo sistema operacional (OS) que contém registros de domínio frequentemente acessados.

Esses registros, conhecidos como *Resource Records (RR)*, incluem informações como:
- Nome do domínio
- Tipo de registro
- Tempo de vida (*Time to Live - TTL*)
- Dados do recurso

### Exemplo de um Resource Record:
<name> <ttl> <class> <type> <rdlength> <radata>

yaml
Copiar código

Quando você acessa um site, o navegador consulta o sistema operacional para recuperar o IP correspondente ao domínio. Se o registro já está no cache local, a resposta é mais rápida. Caso contrário, o sistema realiza uma consulta ao servidor DNS público.

### Problemas no Cache DNS
O cache DNS pode expirar ou corromper, resultando em erros de navegação. Para resolver isso, é necessário limpar (ou *flush*) o cache DNS, permitindo que o sistema operacional crie novos registros.

---

## Find Your Local DNS Resolver

Distribuições Linux usam diferentes resolvers DNS, como `systemd-resolved` ou `dnsmasq`. Para identificar qual resolver está em uso, execute o seguinte comando no terminal:

```bash
sudo lsof -i :53 -S
```
Este comando lista os serviços que estão escutando na porta 53, utilizada pelo DNS.
Ubuntu 18.04 ou superior: Geralmente utiliza o systemd-resolved.
Versões anteriores: Podem usar dnsmasq.
How to Flush the DNS Cache on Linux?
Flush DNS Cache Using systemd-resolved
Se o serviço systemd-resolved está ativo, execute um dos seguintes comandos:

```bash
sudo resolvectl flush-caches
sudo systemd-resolve --flush-caches
```
Para verificar o tamanho atual do cache após o flush, use:

```bash
sudo systemd-resolve --statistics
sudo resolvectl statistics
```

Clear DNS Cache Using Signals
Outra forma de limpar o cache DNS com systemd-resolved é enviar um sinal USR2:

```bash
sudo killall -USR2 systemd-resolved
```
Para verificar o estado atual no journal, envie o sinal USR1:

```bash
sudo killall -USR1 systemd-resolved
sudo journalctl -r -u systemd-resolved
```

Flush DNS Cache Using dnsmasq
Se o sistema utiliza dnsmasq, execute o seguinte comando:

```bash
sudo killall -HUP dnsmasq
```
Para registrar as estatísticas no arquivo de log e verificar o tamanho do cache:

```bash
sudo killall -USR1 dnsmasq
tail -f n1000 /var/log/syslog | grep "cache size"
```
Se o dnsmasq estiver configurado como serviço, reiniciá-lo também limpa o cache:

```bash
sudo systemctl restart dnsmasq
sudo service dnsmasq restart
```
Clear DNS Cache Using nscd in RedHat
No RedHat, o daemon nscd é usado como cache DNS. Para limpar o cache, reinicie o serviço com um dos seguintes comandos:

```bash
sudo systemctl restart nscd.service
sudo service nscd restart
```
Clear DNS Cache on Google Chrome
O navegador Google Chrome também armazena seu próprio cache DNS. Para limpá-lo, siga os passos abaixo:

Abra o Chrome e digite na barra de endereços:
```bash
chrome://net-internals/#dns
```
Clique no botão Clear host cache.

Artigo retirado de https://www.siteground.com/kb/flush-dns-cache-in-linux/
