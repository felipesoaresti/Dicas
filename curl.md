Retirado do site do Joseph Matino: https://josephmatino.com/curl-error-unable-to-get-local-issuer-certificate/

Atualzar a CA-certificates do seu host.    
    
    $ sudo apt-get update && sudo apt-get install --reinstall ca-certificates

Verificar os certificados que o servidor esta enviando 

    $ openssl s_client -connect example.com:443 -showcerts

Baixando o certificado do servidor e inserindo na sua ca

    $ echo | openssl s_client -connect example.com:443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > server.crt
    $ sudo cp server.crt /usr/local/share/ca-certificates/
    $ sudo update-ca-certificates






