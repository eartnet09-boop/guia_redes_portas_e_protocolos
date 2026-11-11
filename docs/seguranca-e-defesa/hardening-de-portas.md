# Hardening de portas

1. **Inventarie:** associe listener, processo, proprietário, ambiente, dependências e exposição. Revalide após mudanças e atualizações.
2. **Remova o desnecessário:** desative serviços, listeners e protocolos legados não usados; não apenas mude a porta padrão.
3. **Reduza alcance:** não exponha banco de dados, SMB, interfaces DevOps ou administração diretamente à internet. Prefira rede privada, VPN/ZTNA, bastion ou proxy reverso adequado ao protocolo.
4. **Proteja identidade e dados:** autenticação forte/MFA onde suportado, menor privilégio, TLS, gestão de segredos e backups testados.
5. **Corrija e valide:** priorize vulnerabilidades exploradas conhecidas no CISA KEV (Cybersecurity and Infrastructure Security Agency Known Exploited Vulnerabilities, catálogo de vulnerabilidades exploradas conhecidas), janela de exposição e criticidade do ativo; verifique serviço e política após a mudança.

Comandos de inspeção local:

```sh
ss -tulpen
sudo lsof -nP -iTCP -sTCP:LISTEN
nmap -sT -sV --version-light -p 22,80,443,445,3389,5432,8080,9200,27017 127.0.0.1
```

```powershell
Get-NetTCPConnection -State Listen | Sort-Object LocalPort
Get-NetUDPEndpoint | Sort-Object LocalPort
```

Nmap (Network Mapper) acima consulta apenas loopback (interface local). VPN (Virtual Private Network) e ZTNA (Zero Trust Network Access) são opções de acesso remoto controlado, não substitutos para autorização e menor privilégio. Use varredura de rede somente com autorização explícita, escopo definido e janela aprovada; confirme impacto com o proprietário do serviço.


**Ação prática:** compare a saída local ao inventário e às regras de entrada/saída; abra exceção apenas com justificativa e prazo de revisão. Referências: [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) e [CIS Controls](https://www.cisecurity.org/controls).
