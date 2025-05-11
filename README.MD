## Roteiro 3:

Aluno: Luís Antonio Santana Rodrigues

----

**O Squid**

O Squid é um servidor proxy que suporta protocolos como HTTP, HTTPS, FTP e Gopher. Ele é amplamente utilizado para melhorar o desempenho de acesso à Internet, economizar largura de banda, gerenciar o uso da rede e aumentar a sua segurança.

Para a atividade, o Squid foi instalado através do comando passado no slide:

```bash
apt install squid
```

----

Foram propostas duas atividades:

1. Exercício 1
- Criar uma regra para bloquear os sites facebook.com e youtube.com para ser aplicado para todas as máquinas da rede.
-  Libere os sites facebook.com e youtube.com para o ip final 100 (192.168.0.100)

**Resolução:**

Foi feita uma cópia do arquivo editado de configuração do Squid, que se encontra em `/etc/squid/squid.conf`, para o arquivo `squid.conf`. Para facilitar a correção, o trecho responsável pela resolução do exercício foi inserido abaixo:
``` bash
    # Exercicio 1:
    # Referencia: https://cloudinfrastructureservices.co.uk/how-to-block-websites-using-squid-proxy-server/
    # https://serverfault.com/questions/1008068/how-to-enable-only-one-ip-to-squid-proxy-server

    # Sites a serem bloqueados
    acl bloqueados dstdomain .facebook.com .youtube.com


    acl ip_do_dono src 192.168.0.100
    http_access allow ip_do_dono bloqueados

    # Bloqueio de domínios HTTP para os demais, para HTTPS precisaria de SSL bump
    # https://wiki.squid-cache.org/Features/SslBump
    http_access deny bloqueados
``` 

2. Exercício 2
- Criar uma regra para bloquear a expressão terra aplicando a toda rede.

- Liberar o site terraviva para toda rede.

**Resolução:**

Foi feita uma cópia do arquivo editado de configuração do Squid, que se encontra em `/etc/squid/squid.conf`, para o arquivo `squid.conf`. Para facilitar a correção, o trecho responsável pela resolução do exercício foi inserido abaixo:
``` bash
    # Exercicio 2:
    # Referencia: https://cloudinfrastructureservices.co.uk/how-to-block-websites-using-squid-proxy-server/
    # Permitir acesso ao site terraviva.com.br
    acl terraviva_liberada dstdomain .terraviva.com.br
    http_access allow terraviva_liberada

    acl terra_bloq url_regex -i ^http://([^/]*\.)*terra[^/]*\.
    http_access deny terra_bloq
```

----

**Observações:**
- As demais regras do arquivo `squid.conf` foram mantidas, apenas foram adicionadas as regras para os exercícios. Como as regras do SQUID são lidas de cima para baixo, as regras foram inseridas no início do arquivo para garantir que elas tivessam prioridade máxima.

- O arquivo completo pode ser encontrado no repositório do GitHub, na pasta `roteiro3` com o nome `squid.conf`.

- Conforme comentado no arquivo, o bloqueio foi feito para as páginas com protocolo HTTP. Para o bloqueio de páginas HTTPS, seria necessário realizar o bloqueio de forma diferente, utilizando o `ssl_bump`, que é um recurso do Squid para inspecionar o tráfego HTTPS. O bloqueio de páginas HTTPS não foi realizado.

**Referências:**

- https://cloudinfrastructureservices.co.uk/how-to-block-websites-using-squid-proxy-server/

- https://serverfault.com/questions/1008068/how-to-enable-only-one-ip-to-squid-proxy-server
