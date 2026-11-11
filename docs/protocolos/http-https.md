# HTTP e HTTPS

HTTP (Hypertext Transfer Protocol) define mensagens de requisição e resposta. RFC (Request for Comments) 9112 especifica HTTP/1.1, [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110) a semântica, [RFC 9113](https://www.rfc-editor.org/rfc/rfc9113) HTTP/2 e [RFC 9114](https://www.rfc-editor.org/rfc/rfc9114) HTTP/3. HTTPS é HTTP protegido por TLS (Transport Layer Security); HTTP/3 usa QUIC (Quick UDP Internet Connections) sobre UDP (User Datagram Protocol).

```mermaid
sequenceDiagram
  participant C as Cliente
  participant P as Proxy / balanceador
  participant A as Aplicação
  C->>P: conexão e requisição (método, Host, caminho)
  P->>A: requisição encaminhada
  A-->>P: status, headers e corpo
  P-->>C: resposta; logs correlacionados por request ID
  Note over P,A: parser divergente pode causar request smuggling
```

Métodos comuns incluem GET, POST, PUT, PATCH e DELETE; não representam autorização. Status 401/403 sinalizam decisões de acesso, mas não demonstram por si só controles corretos; 429 pode indicar rate limit; 5xx pode indicar falha. Host e `:authority` influenciam roteamento, logo devem ser validados.

## Ataques e detecção

- **Request smuggling:** parsers intermediário e backend discordam sobre limites/framing da mensagem. Indicadores: respostas fora de ordem, discrepâncias de parsing e padrões anômalos em `Content-Length`/`Transfer-Encoding`. Normalize/rejeite mensagens ambíguas e mantenha componentes atualizados.
- **Host header injection:** confiança indevida no Host para gerar links, redirects ou reset de senha. Indicador: Host não permitido ou inconsistência com SNI/roteamento. Use allowlist de hosts canônicos.
- **Abuso de aplicação:** autenticação fraca, falhas de autorização, APIs expostas e ataques automatizados. Correlacione identidade, rota, método, status, latência e taxa; evite registrar segredos ou dados pessoais.

Monitore método, host, caminho normalizado, status, request ID (identificador da requisição), usuário pseudonimizado, origem conforme política e latência. API (Application Programming Interface, interface de programação de aplicações) deve ter controles de acesso próprios. Considere proxies confiáveis ao interpretar `X-Forwarded-For`.


**Ação prática:** valide Host, alinhe parsing em proxy/backend, limite taxa de forma contextual e mantenha HTTP protegido por TLS. Links: [IANA HTTP Fields](https://www.iana.org/assignments/http-fields/) e [OWASP HTTP Request Smuggling](https://owasp.org/www-community/attacks/HTTP_Request_Smuggling).
