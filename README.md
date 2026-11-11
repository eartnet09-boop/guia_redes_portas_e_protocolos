# Redes, Portas e Protocolos — Guia de Segurança Defensiva

[![Licença MIT](https://img.shields.io/badge/licen%C3%A7a-MIT-green.svg)](LICENSE)
[![Revisado em 2026-11-11](https://img.shields.io/badge/revis%C3%A3o-2026--11--11-blue.svg)](docs/ataques-recentes/panorama-2025-2026.md)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK-red.svg)](https://attack.mitre.org/)

Referência prática para entender a exposição de serviços de rede, priorizar riscos e aplicar controles de Blue Team. Os comandos são voltados à inspeção de sistemas próprios ou explicitamente autorizados. Este guia não substitui inventário, gestão de vulnerabilidades nem análise contextual.

MITRE significa MITRE Corporation; ATT&CK é a base de táticas e técnicas adversárias mantida pela MITRE. SOC (Security Operations Center) é o Centro de Operações de Segurança.

## Navegação rápida

| Pergunta | Onde ir |
|---|---|
| Qual a diferença entre porta e protocolo? | [Portas vs. protocolos](docs/fundamentos/portas-vs-protocolos.md) |
| Quais portas exigem atenção prioritária? | [Portas críticas](docs/mapa-de-portas/portas-criticas.md) |
| Como reduzir a exposição de SMB/445? | [Portas críticas](docs/mapa-de-portas/portas-criticas.md) e [SMB](docs/protocolos/smb.md) |
| Que ataques e CVEs recentes acompanhar? | [Panorama 2025–2026](docs/ataques-recentes/panorama-2025-2026.md) |
| Como segmentar a rede? | [Segmentação e firewall](docs/seguranca-e-defesa/segmentacao-e-firewall.md) |
| Onde estão os padrões e catálogos oficiais? | [Links oficiais](data/links-oficiais.md) |

## Comunicação e pontos de controle

```mermaid
flowchart LR
  A[Aplicação: HTTP, DNS, SMB] -->|dados da aplicação| T[Transporte: TCP ou UDP]
  T -->|porta de origem e destino| N[IP: endereçamento e roteamento]
  N --> L[Enlace: Ethernet ou Wi-Fi]
  subgraph Controles de defesa
    F[Firewall e ACL: fluxo permitido]
    S[Segmentação: limitar alcance]
    M[EDR, NDR e logs: detectar e responder]
  end
  F -. política por fluxo .-> T
  S -. zonas de confiança .-> N
  M -. telemetria correlacionada .-> A
```

Uma porta identifica um endpoint de transporte; não prova qual serviço está ativo nem se ele está vulnerável. Veja [fundamentos](docs/fundamentos/portas-vs-protocolos.md), [glossário](docs/glossario.md) e [mapa de portas](docs/mapa-de-portas/portas-criticas.md).

## Quick Start

1. Consulte o [glossário](docs/glossario.md) para siglas e conceitos.
2. Leia [portas vs. protocolos](docs/fundamentos/portas-vs-protocolos.md).
3. Use o [mapa de portas críticas](docs/mapa-de-portas/portas-criticas.md) para priorizar controles.

## Aviso de uso


Execute verificações apenas em ativos próprios ou com autorização documentada. Não use resultados de uma porta isolada para concluir que um sistema está comprometido: confirme serviço, versão, exposição, configuração e evidências nos logs. Os dados de incidentes devem ser conferidos nas fontes vinculadas; alegações sem referência rastreável são identificadas como não confirmadas.
