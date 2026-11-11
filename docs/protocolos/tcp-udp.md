# TCP e UDP

TCP (Transmission Control Protocol) é um transporte orientado a conexão, com sequência, confirmação e retransmissão. UDP (User Datagram Protocol) envia datagramas sem estabelecer uma conexão equivalente; aplicações podem implementar confiabilidade e controle por conta própria. Referências: [RFC 9293 (TCP)](https://www.rfc-editor.org/rfc/rfc9293) e [RFC 768 (UDP)](https://www.rfc-editor.org/rfc/rfc768).

## Fluxo TCP e superfície

```mermaid
sequenceDiagram
  participant C as Cliente
  participant F as Firewall / sensor
  participant S as Serviço: porta destino
  C->>F: SYN, IP e porta destino
  F->>S: permitido pela política
  S-->>C: SYN-ACK
  C->>S: ACK; conexão estabelecida
  Note over F,S: SYN excessivo pode indicar varredura ou abuso de recursos
```

O handshake não autentica o usuário nem valida a segurança da aplicação. UDP não tem handshake TCP; filtros stateful inferem fluxos, e protocolos como QUIC implementam seus próprios mecanismos.

## Relevância, ataques e monitoramento

- **Varredura/reconhecimento:** muitas tentativas em portas/hosts distintos. Indicador: alta cardinalidade de destino, SYN sem conclusão ou respostas ICMP/RESET; agregue por origem e janela temporal.
- **SYN flood / exaustão:** volume anormal de inícios TCP ou recursos; avalie taxa, distribuição, estado do servidor e mitigação upstream.
- **Abuso/amplificação UDP:** resposta maior que requisição em serviços mal configurados, com origem falsificada. Mitigue com configuração correta, rate limiting e filtragem anti-spoofing na borda.

Monitore 5-tuplas (IP/porta origem e destino, protocolo), flags TCP, conexões incompletas, retransmissões, resets, bytes/pacotes por fluxo e taxas por janela. NAT e proxies alteram a atribuição de origem; correlacione com logs do serviço.


**Ação prática:** compare listeners e fluxos permitidos ao inventário e baseline. Em máquina própria: `ss -tuln`; no Windows: `netstat -ano`. Ver [monitoramento](../seguranca-e-defesa/monitoramento-e-deteccao.md).
