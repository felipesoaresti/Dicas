Exemplo curl.cfg

    forum.checkmk.com|curl_item_1|no_regex|
    your-checkmk.yourdomain.com|curl_item_2|case_multiline|
    checkmk.com|curl_item_3|nocase_nomultiline|

Em cada linha você tem os três campos service_name, curl_item_# e regex_options. Esses campos são separados por |. Não são permitidos comentários ou linhas vazias.

Você precisa criar alguns arquivos adicionais no subdiretório curl no diretório de configuração do agente check_mk (Linux: /etc/check_mk/curl, Windows: C:\ProgramData\checkmk\agent\config\curl). curl_item_# é igual à segunda opção de cada linha no arquivo de configuração curl.cfg

curl_item_#.options → opções cURL linha por linha

Aqui um exemplo desse arquivo de opções

    Windows:

    PS C:\ProgramData\checkmk\agent\config\curl> cat .\curl_item_2.options

        # Created by Check_MK Agent Bakery.
        # This file is managed via WATO, do not edit manually or you
        # lose your changes next time when you update the agent.

        --url "https://your-checkmk.yourdomain.com"
        --compressed
        --connect-timeout 3
        --user-agent "('user_agent', 'curl-checkmk')"
        --location
        --cacert C:/ProgramData/checkmk/agent/config/curl/curl_item_2.ca_cert
        --output NUL
        --header @C:/ProgramData/checkmk/agent/config/curl/curl_item_2.header
    
    Linux:

    cat /etc/check_mk/curl/curl_item_1.options

        # Created by Check_MK Agent Bakery.
        # This file is managed via WATO, do not edit manually or you
        # lose your changes next time when you update the agent.

        --url "https://your-wiki.yourdomain.com/mediawiki"
        --compressed
        --connect-timeout 10
        --user-agent "('user_agent', 'curl-checkmk')"
        --location
        --head
        --cacert /etc/check_mk/curl/curl_item_1.ca_cert 
        --output /var/tmp/curl_output

