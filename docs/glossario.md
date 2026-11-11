# Glossário

Siglas são expandidas na primeira ocorrência deste arquivo. Veja também [categorias de portas IANA](fundamentos/categorias-iana.md) e [TCP/UDP](protocolos/tcp-udp.md).

| Termo | Significado | Relevância para defesa |
|---|---|---|
| ACL | Access Control List, lista de controle de acesso | Regras para permitir ou negar fluxos. |
| ATT&CK | Adversarial Tactics, Techniques, and Common Knowledge | Base da MITRE para descrever comportamento adversário, não severidade de CVE. |
| CPE | Common Platform Enumeration | Identificador padronizado de produtos e versões; útil para inventário e correlação de vulnerabilidades. |
| CVE | Common Vulnerabilities and Exposures | Identificador público de vulnerabilidade; não informa sozinho exposição ou explorabilidade local. |
| CVSS | Common Vulnerability Scoring System | Métrica de severidade técnica; priorize também exposição e exploração confirmada. |
| DNS | Domain Name System | Serviço de nomes; consulte [DNS](protocolos/dns.md). |
| DoH / DoT | DNS over HTTPS / DNS over TLS | DNS protegido por HTTPS ou TLS; pode reduzir visibilidade de rede sem política e telemetria adequadas. |
| EDR / NDR | Endpoint / Network Detection and Response | Telemetria e resposta em endpoints / rede. |
| IANA | Internet Assigned Numbers Authority | Mantém registros de parâmetros, incluindo portas; ver [categorias](fundamentos/categorias-iana.md). |
| IP | Internet Protocol | Endereçamento e roteamento entre redes. |
| KEV | Known Exploited Vulnerabilities | Catálogo da CISA de vulnerabilidades exploradas conhecidas. |
| MSSP | Managed Security Service Provider | Provedor de serviços gerenciados de segurança. |
| MSRPC | Microsoft Remote Procedure Call | Família de chamadas remotas; o endpoint mapper TCP/135 pode anunciar endpoints dinâmicos. |
| NSG | Network Security Group | Conjunto de regras de rede em plataformas de nuvem, com nomenclatura variável por provedor. |
| RFC | Request for Comments | Série de documentos técnicos; nem todo RFC é padrão final. |
| RCE | Remote Code Execution | Execução remota de código; impacto depende de condições e privilégio. |
| SMB | Server Message Block | Compartilhamento de arquivos e outros recursos, comumente em TCP/445. |
| SNI | Server Name Indication | Extensão TLS que normalmente revela o nome pretendido do servidor durante o handshake. |
| SOC | Security Operations Center | Centro de operações de segurança. |
| TCP / UDP | Transmission Control Protocol / User Datagram Protocol | Transportes com propriedades diferentes; [detalhes](protocolos/tcp-udp.md). |
| TLS | Transport Layer Security | Proteção criptográfica de canais; [detalhes](protocolos/tls.md). |
| VLAN | Virtual Local Area Network | Segmentação lógica de camada 2; não substitui firewall entre zonas. |


**Ação prática:** associe cada alerta de porta a ativo, proprietário, serviço, exposição e regra de negócio; o número sozinho não identifica o risco.
