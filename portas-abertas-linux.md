Como Verificar as Portas Abertas no Linux

Para verificar as portas em aberto com o netstat, as opções “-tunl” podem ser usadas:

    -t para TCP
    -u para UDP
    -n para não resolver nomes
    -l para as portas abertas (listen)

Neste exemplo, o netstat lista as portas abertas TCP/UDP:

    $ netstat -tunl

O netstat com a opção -p mostra os processos donos das portas. Para utilizar essa opção é preciso ser o root:

    $ sudo netstat -tunlp

O comando ss também pode mostrar as portas abertas com a opção idêntica:

    $ ss -tunelp

A opção “-i” do lsof filtra os arquivos em aberto do tipo de endereços de Internet. É necessário executar o lsof como root:

    $ sudo lsof -i

O comando “fuser” também pode ser usado para mostrar informações sobre uma determinada porta em aberto. Ele identifica os processos através dos arquivos ou sockets, retornando o PID dos processos:

    $ sudo fuser 22/tcp

Para saber qual processo está usando determinado arquivo:

    $ fuser -v /bin/bash
