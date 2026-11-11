# TLS

TLS (Transport Layer Security) protege confidencialidade e integridade de canais. TLS 1.2 é especificado na [RFC 5246](https://www.rfc-editor.org/rfc/rfc5246); TLS 1.3, na [RFC 8446](https://www.rfc-editor.org/rfc/rfc8446). Use versões e suites suportadas pela política atual; TLS não corrige falhas da aplicação nem garante que o servidor seja benigno.

```mermaid
sequenceDiagram
  participant C as Cliente
  participant S as Servidor: porta do serviço
  C->>S: ClientHello, versões, suites, SNI
  S-->>C: ServerHello e parâmetros negociados
  S-->>C: certificado e prova de posse da chave
  C->>C: valida cadeia, nome e validade
  C->>S: Finished; dados da aplicação cifrados
  Note over C,S: TLS 1.3 reduz o handshake; 0-RTT tem riscos de replay
```

TLS 1.2 e 1.3 diferem em negociação e mensagens; em ambos a validação do certificado no cliente é essencial. SNI (Server Name Indication) normalmente revela o nome pretendido no ClientHello; ECH (Encrypted Client Hello) pode proteger esse metadado quando suportado. TLS cifra conteúdo, ocultando do sensor passivo corpo, caminhos HTTP (Hypertext Transfer Protocol) e credenciais, embora IP (Internet Protocol), porta, volume, tempo e por vezes SNI permaneçam visíveis.

## Riscos e monitoramento

- **Certificado inválido/expirado ou nome incompatível:** acompanhar validade, cadeia, emissor e mudanças inesperadas.
- **Downgrade/configuração fraca:** registrar versão negociada, suites, falhas de handshake e servidores legados.
- **Inspeção TLS:** proxy de inspeção pode ampliar risco de privacidade e concentrar chaves; governança, escopo e retenção precisam ser aprovados.
- **0-RTT (zero round-trip time):** aplicações com operações não idempotentes devem considerar replay (repetição de requisição).

Monitore inventário de certificados, handshake failures, versões, SNI quando disponível e correlação com logs de aplicação. Não inferir conteúdo malicioso apenas de TLS cifrado.


**Ação prática:** automatize alerta de expiração/alteração de certificado, remova versões legadas conforme compatibilidade e teste validação de cadeia. Links: [IANA TLS Parameters](https://www.iana.org/assignments/tls-parameters/), [RFC 6066 (SNI)](https://www.rfc-editor.org/rfc/rfc6066).
