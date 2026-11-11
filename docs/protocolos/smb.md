# SMB

SMB (Server Message Block) oferece compartilhamento de arquivos e outros recursos. SMB 2.x/3.x é a família atual; SMBv1 é legado e deve ser removido quando não houver dependência justificada. A especificação do protocolo é mantida pela Microsoft em [MS-SMB2](https://learn.microsoft.com/openspecs/windows_protocols/ms-smb2/). SMB direto sobre TCP usa normalmente 445; NetBIOS legado pode envolver 137–139.

```mermaid
sequenceDiagram
  participant C as Cliente
  participant FW as Firewall / segmentação
  participant S as Servidor SMB: TCP/445
  C->>FW: conexão TCP
  FW->>S: permitir somente origem autorizada
  C->>S: negociação SMB e capacidades
  C->>S: autenticação e acesso ao compartilhamento
  S-->>C: dados; sensor observa sessão e autenticação
  Note over FW,S: exploração, credencial roubada ou share amplo aumentam impacto
```

## Segurança, ataques e detecção

- **Exploração de SMB legado:** EternalBlue/CVE-2017-0144 afetou implementações vulneráveis de SMBv1; WannaCry demonstrou o impacto de sistemas sem correção e rede plana. Isso não significa que SMB moderno seja vulnerável à mesma falha. Consulte [CVE-2017-0144](https://nvd.nist.gov/vuln/detail/CVE-2017-0144).
- **Movimento lateral e abuso de credencial:** acessos incomuns a vários hosts, logins falhos seguidos de sucesso, criação de serviço/compartilhamento ou escrita em shares administrativos merecem correlação.
- **Assinatura e criptografia:** assinatura SMB ajuda a proteger integridade/autenticidade de mensagens; criptografia SMB protege conteúdo em trânsito. Exigências devem ser testadas quanto a compatibilidade e performance.

Monitore versão negociada, autenticação, origem/destino, compartilhamento acessado, falhas, volume e operações sensíveis. No Windows, revisar `Get-SmbServerConfiguration`, `Get-SmbShare` e logs SMB; alertar sobre SMBv1 habilitado e conexões de segmentos de usuário para estações/servidores não previstos.


**Ação prática:** desabilite SMBv1, aplique atualizações, restrinja TCP/445 entre zonas, aplique menor privilégio e configure assinatura/criptografia conforme risco. Nunca exponha SMB à internet. Ver [portas críticas](../mapa-de-portas/portas-criticas.md).
