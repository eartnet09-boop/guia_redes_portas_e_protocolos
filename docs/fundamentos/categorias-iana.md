# Categorias de portas IANA

A Internet Assigned Numbers Authority (IANA) organiza as portas de transporte em faixas descritas na [RFC 6335](https://www.rfc-editor.org/rfc/rfc6335) e no [registro oficial](https://www.iana.org/assignments/service-names-port-numbers/).

| Categoria | Faixa | Exemplos | Implicação de segurança |
|---|---:|---|---|
| System / Well-Known | 0–1023 | DNS 53, HTTP 80, SSH 22, HTTPS 443 | Serviços comuns e previsíveis são muito monitorados; privilégios para bind variam por sistema. |
| User / Registered | 1024–49151 | MSSQL 1433, PostgreSQL 5432, RDP 3389 | Serviços de negócio podem ficar expostos por configuração; confira o serviço real e a atribuição IANA. |
| Dynamic / Private | 49152–65535 | Portas efêmeras de cliente; endpoints RPC em algumas plataformas | Faixa frequentemente usada para conexões de saída e serviços dinâmicos. Não bloqueie ou permita indiscriminadamente. |

Os limites são convenções IANA; sistemas podem configurar outras faixas para portas efêmeras. Uma atribuição no registro não garante que o tráfego observado use aquele protocolo. As atribuições são específicas ao transporte (TCP, UDP, etc.).


**Ação prática:** consulte o registro para a atribuição e valide o processo/socket no host. Em regras de firewall, documente direção, transporte, origem, destino, proprietário e justificativa; trate faixas dinâmicas com política explícita.
