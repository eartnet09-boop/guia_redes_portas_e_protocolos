# Portas de bancos de dados

| Porta → protocolo(s) → serviço | Risco | Vetor comum | Verificação Blue Team | Ação defensiva |
|---|---|---|---|---|
| **1433/TCP → TDS** (Tabular Data Stream) → Microsoft SQL Server (MSSQL) | Alto; crítico se exposto com credenciais fracas | Tentativas de autenticação, contas excessivas, serviço vulnerável ou acesso a backup/dados | Windows: `Get-NetTCPConnection -LocalPort 1433 -State Listen`; inventariar listeners da instância | Acesso apenas por aplicações/bastions previstos; TLS (Transport Layer Security); MFA (autenticação multifator)/identidades gerenciadas quando possível; menor privilégio, patches e auditoria. |
| **3306/TCP → protocolo MySQL** → MySQL/MariaDB | Alto; crítico se internet-facing | Credenciais reutilizadas, configuração permissiva, serviço sem atualização e exposição de dados | `ss -lntp '( sport = :3306 )'`; revisar `bind-address` e grants | Rede privada, origem restrita, TLS, contas por aplicação e menor privilégio; não usar conta administrativa pela aplicação. |
| **5432/TCP → protocolo PostgreSQL** → PostgreSQL | Alto; crítico se acesso amplo | Regras `pg_hba.conf` permissivas, credenciais fracas, exploração de versão vulnerável ou abuso de privilégios | `ss -lntp '( sport = :5432 )'`; revisar `listen_addresses` e `pg_hba.conf` | Restringir redes e métodos de autenticação, TLS, patching, roles mínimas, logs e backups testados. |
| **27017/TCP → protocolo MongoDB** → MongoDB | Crítico se sem autenticação ou exposto | Leitura/alteração/exclusão não autorizada por exposição ou controle de acesso incorreto | `ss -lntp '( sport = :27017 )'`; revisar autenticação, bind e firewall | Bind privado, autenticação, RBAC (Role-Based Access Control, controle de acesso baseado em funções), TLS, segmentação, backups imutáveis e verificação externa autorizada. |

Banco acessível pela internet amplia a superfície de ataque e não deve ser a arquitetura padrão. Portas não substituem autenticação: use ACLs de rede e controles da própria base. Não confie em “porta não padrão” como defesa.


**Ação prática:** compare listeners com o inventário e regras de entrada/saída; remova exposição pública, valide autenticação e privilégios e teste recuperação de backup. Para outras medidas, veja [hardening](../seguranca-e-defesa/hardening-de-portas.md).
