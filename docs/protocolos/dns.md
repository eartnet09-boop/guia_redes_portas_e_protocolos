# DNS

DNS (Domain Name System) traduz nomes em dados como endereços IP (Internet Protocol). O modelo base usa UDP (User Datagram Protocol) e TCP (Transmission Control Protocol)/53; TCP também é necessário para respostas truncadas e outras operações. DNSSEC (Domain Name System Security Extensions) autentica origem e integridade de dados assinados, mas não cifra consultas. DoT (DNS over TLS, Transport Layer Security) e DoH (DNS over HTTPS, HTTP Secure) protegem o transporte até o resolvedor, não tornam o nome confiável. Referências: [RFC 1034](https://www.rfc-editor.org/rfc/rfc1034), [RFC 1035](https://www.rfc-editor.org/rfc/rfc1035), [RFC 4033](https://www.rfc-editor.org/rfc/rfc4033), [RFC 7858](https://www.rfc-editor.org/rfc/rfc7858), [RFC 8484](https://www.rfc-editor.org/rfc/rfc8484).

```mermaid
sequenceDiagram
  participant C as Cliente
  participant R as Resolvedor recursivo: 53
  participant A as Autoritativo: 53
  C->>R: consulta nome/tipo
  R->>A: consulta se não estiver em cache
  A-->>R: resposta e dados DNSSEC, quando assinados
  R-->>C: resposta; sensor observa metadados
  Note over C,R: DoT/DoH cifram o trecho até o resolvedor escolhido
```

## Riscos, ataques e indicadores

- **Amplificação/reflection:** resolvedores abertos ou serviços mal configurados respondem a terceiros; indicador: volume/respostas DNS anormais. Restrinja recursão e aplique controles de saída/anti-spoofing.
- **Túnel DNS:** dados codificados em consultas/respostas podem elevar subdomínios únicos, comprimento, entropia e volume. Nenhum indicador isolado confirma exfiltração; correlacione processo, domínio, frequência e destino.
- **Envenenamento/alteração de resposta:** valide DNSSEC onde aplicável e proteja resolvedores, zonas e credenciais de registrador.
- **DoH/DoT não gerenciado:** reduz visibilidade do DNS corporativo; política explícita de resolvedores e telemetria de endpoint ajudam a manter controle sem inspecionar conteúdo indiscriminadamente.

Monitore qname (nome consultado)/tipo/rcode (código de resposta), resolvedor, volume, respostas NXDOMAIN (nome inexistente), domínios novos, TTL (Time to Live, tempo de vida) incomum, registros alterados e processo cliente. Minimize e proteja dados de consulta por privacidade.


**Ação prática:** inventarie resolvedores permitidos, desative recursão aberta, proteja atualizações de zona e alerte sobre consultas anômalas. Referências oficiais adicionais: [IANA DNS Parameters](https://www.iana.org/assignments/dns-parameters/).
