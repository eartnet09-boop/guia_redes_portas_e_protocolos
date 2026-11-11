# Monitoramento e detecção

Comece pelo inventário de ativos, baseline de listeners e fluxos esperados. EDR (Endpoint Detection and Response, detecção e resposta em endpoints) e NDR (Network Detection and Response, detecção e resposta em rede) agregam telemetria complementar. Um alerta ganha valor quando correlaciona host, processo, usuário, direção, exposição e contexto temporal.

## Verificação local

```sh
ss -tuln
sudo lsof -nP -i
nmap -sT -sV --version-light -p 22,80,443,445,3389,5985,5986,5432,8080,9200,27017 127.0.0.1
```

```powershell
netstat -ano
Get-NetTCPConnection -State Listen
```

O exemplo Nmap é restrito ao loopback. Não use contra sistemas sem autorização documentada. `netstat -ano` lista portas, conexões e PID; associe PID ao processo antes de concluir.

## Indicadores e telemetria

| Sinal | Dados para correlacionar |
|---|---|
| Varredura horizontal/vertical | origem, destinos, portas distintas, taxa, SYN (Synchronize, flag inicial TCP) sem sessão e respostas. |
| Tentativas de conexão fora do padrão | segmento, horário, processo, regra acionada, resultado e repetição. |
| Acesso administrativo atípico | identidade, origem, MFA, sucesso/falha, host-alvo e sessão subsequente. |
| Serviço inesperado | listener novo, binário/assinatura, usuário do processo, mudança de configuração e exposição. |
| DNS ou TLS anômalo | resolvedor/SNI quando disponível, volume, domínio novo, falhas de handshake e processo cliente. |

Filtros Wireshark úteis para triagem: `tcp.flags.syn == 1 && tcp.flags.ack == 0`, `tcp.port == 445`, `udp.port == 53`, `tls.handshake.type == 1`. Filtros dependem de descriptografia e dissectors; ausência de conteúdo visível não é prova de ausência de ameaça.

Retenha logs conforme política, restrinja acesso e minimize dados pessoais. Correlacione firewall, DNS, proxy, autenticação, EDR e sistema operacional; preserve relógios sincronizados.


**Ação prática:** gere baseline autorizado, alerte sobre listeners novos e conexões administrativas inesperadas e valide cada alerta no host e no firewall antes de responder.
