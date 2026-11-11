# Portas de acesso remoto

| Porta → protocolo(s) → serviço | Risco | Vetor comum | Verificação Blue Team | Ação defensiva |
|---|---|---|---|---|
| **22/TCP → SSH** (Secure Shell) → shell e transferência segura | Alto quando exposto; crítico em contas privilegiadas sem MFA | Tentativas de senha, credenciais/chaves expostas, configuração fraca ou versão vulnerável. Alegações de “639 milhões de sessões em 162 dias” não são reproduzidas aqui sem relatório primário verificável, metodologia e período. | `ss -lntp '( sport = :22 )'`; revisar logs `journalctl -u ssh` (ou `sshd`) | Chaves protegidas, MFA quando disponível, contas nominativas, desativar login root/senha se compatível, limitar origem e monitorar falhas. |
| **3389/TCP → RDP** → área de trabalho remota Windows | Crítico se diretamente exposto | Password spraying, abuso de credencial e exploração de falhas em sistemas não corrigidos. | `Get-NetTCPConnection -LocalPort 3389 -State Listen`; revisar Security Event Log | VPN/ZTNA ou gateway, MFA, NLA, atualização, política de bloqueio e alertas; ver [portas críticas](portas-criticas.md). |
| **5900/TCP → VNC** (Virtual Network Computing) → console remoto | Alto se exposto; risco maior sem autenticação forte ou criptografia | Tentativa de senha, credenciais padrão e captura de sessão em implementação/configuração insegura. | `ss -lntp '( sport = :5900 )'`; inventariar serviço e configuração de TLS/autenticação | Não expor à internet; encapsular em VPN, exigir autenticação robusta e criptografia, segmentar e desativar quando não usado. |

Não há, nesta documentação, uma fonte primária rastreável para a alegação de que VNC seria a “terceira porta mais visada” ou para a métrica de sessões SSH mencionada em rascunhos. Não tratamos essas afirmações como fatos. Consulte o [MITRE ATT&CK](https://attack.mitre.org/) e [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) para acompanhar técnicas e vulnerabilidades exploradas.


**Ação prática:** mantenha inventário de sessões administrativas, correlacione falhas e logins bem-sucedidos, e restrinja a origem dessas portas a bastions ou sub-redes administrativas.
