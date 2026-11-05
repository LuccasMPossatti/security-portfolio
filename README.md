# Security Portfolio — Luccas M. Possatti

**Infraestrutura e NOC como base para segurança defensiva e gestão de riscos.**

Sou profissional de TI com experiência em infraestrutura, suporte N1/N2 e monitoramento NOC. Minha trajetória inclui gestão de incidentes, investigação de causa raiz e observabilidade com Zabbix e Dynatrace, além de contato com ambientes cloud e automação industrial.

Estou direcionando esse repertório para **SOC / Blue Team e GRC**, com estudos práticos que conectam eventos técnicos, evidências e decisões de risco. Meu interesse de longo prazo é segurança **OT/ICS**, aproveitando a experiência com automação.

## Projeto em destaque

### [Detecção e investigação de tentativas de senha SSH com Wazuh](01-ssh-bruteforce-wazuh/README.md)

Laboratório com **três máquinas virtuais**, Linux, OpenSSH e Wazuh. O estudo acompanha uma tentativa controlada de adivinhação de senha desde os eventos de autenticação até a correlação no SIEM, a investigação e a análise de riscos.

**Resultado demonstrado:** oito falhas SSH correlacionadas pela regra 5763, nível 10. O trabalho reúne investigação de autenticação, reconciliação de alertas e tradução dos achados em riscos e tratamentos propostos.

**[Explorar o estudo](01-ssh-bruteforce-wazuh/README.md)** · **[Ler a investigação](01-ssh-bruteforce-wazuh/docs/incident-report.md)** · **[Consultar os riscos](01-ssh-bruteforce-wazuh/docs/fase11-grc-risco-controles.md)**

| Área | Entrega |
|---|---|
| SOC | Investigação com timeline, análise de alertas e limites das conclusões |
| GRC | Registro de riscos, justificativas e plano de tratamento |
| Referenciais | Mapeamento analítico para MITRE ATT&CK, NIST CSF 2.0, NIST SP 800-53 Rev. 5 e CIS Controls v8.1 |
| Evidências | Extratos analíticos anonimizados, índice de fontes e integridade dos documentos públicos |

**Tecnologias:** Wazuh · Linux · OpenSSH · VirtualBox · Hydra (simulação autorizada).

Os documentos distinguem observações, interpretações e recomendações. O escopo demonstrado é detecção e investigação; prevenção e contenção ficam como tratamentos propostos. Os limites das fontes estão registrados no relatório.

## Competências colocadas em prática

| Base de operações | Aplicação no projeto |
|---|---|
| Monitoramento e troubleshooting | Verificação de agente, eventos, regras e correlação |
| Administração Linux e redes | OpenSSH, serviços, endereçamento e alcance das VMs |
| Gestão de incidentes | Timeline, hipóteses, investigação e comunicação do resultado |
| Análise de risco | Cenários, premissas, controles e priorização de tratamento |

## Cursos e estudos

| Situação | Formação complementar |
|---|---|
| Concluído | TryHackMe — SOC Level 1 |
| Concluído | Getting Started with Cybersecurity — IBM SkillsBuild |
| Concluído | Segurança em TI — Fundação Bradesco |
| Histórico de formação | Apple Service Mac ACMT (2019) e iOS ACiT (2018) |
| Em estudo | CompTIA Security+; NIST CSF; fundamentos de segurança OT/ICS e IEC 62443 |

O estudo publicado é um projeto pessoal de laboratório. As experiências profissionais, os cursos e a prática de laboratório são apresentados em seus respectivos contextos.

## Próximas frentes

Melhorias de cobertura e resposta no laboratório atual; no longo prazo, estudos de detecção em ambientes IT/OT. Essas frentes são planejamento, não resultados atribuídos a este projeto.

## Contato

[LinkedIn](https://www.linkedin.com/in/luccaspossatti/) · [GitHub](https://github.com/LuccasMPossatti)
