# Mapeamento analítico — ATT&CK, NIST e CIS

**Revisão de fontes:** 05/11/2026 · **Escopo:** eventos SSH e controles do laboratório

**Revisão de redação e limites:** 06/11/2026.

As associações abaixo são análise do autor, apoiada por fontes oficiais. Não são equivalências oficiais entre frameworks nem declaração de conformidade. **Demonstrado no recorte** significa que uma atividade foi evidenciada; não significa atendimento integral a um controle organizacional.

## 1. MITRE ATT&CK Enterprise

Referências consultadas: [T1110 — Brute Force](https://attack.mitre.org/techniques/T1110/) (objeto versão 2.8) e [T1110.001 — Password Guessing](https://attack.mitre.org/techniques/T1110/001/) (objeto versão 1.7), ambas com modificação indicada em 12/05/2026. Esses números são versões dos objetos, não a versão do software Wazuh.

| Identificador | Relação com o caso | Evidência / distinção |
|---|---|---|
| TA0006 — Credential Access | Tática associada à tentativa de acesso por senha | Classificação ATT&CK; não prova obtenção de credencial |
| T1110 — Brute Force | Mapeamento automático do alerta 5763 | E35, observado nos metadados |
| T1110.001 — Password Guessing | Mapeamento automático em alertas individuais 5760; comportamento compatível com a sequência registrada | SUP e E34 mostram o ID; E36 apoia a interpretação do método |

Os alertas têm metadados distintos: 5763 traz T1110, enquanto 5760 traz T1110.001. Não se deve generalizar os metadados de um deles ao outro. Nem a classificação identifica o Hydra, nem um ID de acesso remoto comprova movimentação lateral bem-sucedida.

Senhas fortes, MFA e políticas de uso de conta são possíveis medidas para discutir; o experimento não as implementou nem mediu sua eficácia.

## 2. NIST CSF 2.0

Fonte: [NIST CSWP 29, apêndice A](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf), pp. 19–22 da numeração impressa. Os temas são resumos em português.

| Subcategoria | Tema | Relação no laboratório |
|---|---|---|
| ID.RA-04 | Probabilidades e impactos | Discussão qualitativa, com incertezas explícitas |
| ID.RA-05 | Risco inerente e priorização | Relacionada ao método GRC; cenário inerente não medido |
| ID.RA-06 | Tratamento de risco | Plano proposto, sem execução ou acompanhamento demonstrado |
| PR.AA-03 | Autenticação | Parcial: autenticações aceitas e rejeitadas; política completa não avaliada |
| PR.IR-01 | Proteção de redes e ambientes | Relação parcial: topologia descrita, mas limites de entrada na data do incidente não comprovados; proteção integral da rede não demonstrada |
| DE.CM-03 | Monitoramento de atividade | Demonstrado no recorte SSH |
| DE.CM-09 | Monitoramento de sistemas e dados | Parcial: eventos SSH; não se afirma cobertura integral do host |
| DE.AE-02 | Análise de eventos | Demonstrada na investigação |
| DE.AE-03 | Correlação entre fontes | Parcial: SSH e sessões, sem independência total |
| RS.AN-03 | Análise do incidente | Demonstrada no exercício controlado |
| RS.MI-01 | Contenção | Não demonstrada |

Monitorar SSH não estabelece atendimento integral a todas as funções do CSF. Governança organizacional e recuperação não foram exercitadas. A análise de risco não constitui um Organizational Profile formal.

## 3. NIST SP 800-53 Rev. 5

Fonte de texto consultada: [publicação Rev. 5, atualização de dezembro de 2020](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf). A [página do NIST](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) registra releases posteriores, incluindo 5.2.0. Esta tabela é um mapeamento limitado aos identificadores abaixo, não uma auditoria de um baseline atualizado.

| Controle | Título oficial | Associação e limite |
|---|---|---|
| AC-7 | Unsuccessful Logon Attempts | Lacuna de controle entre conexões; política completa não avaliada |
| AU-2 | Event Logging | Relacionado aos eventos SSH registrados; seleção organizacional não definida |
| AU-6 | Audit Record Review, Analysis, and Reporting | Revisão e relatório pontuais demonstrados |
| AU-11 | Audit Record Retention | Retenção suficiente não estabelecida |
| AU-12 | Audit Record Generation | Geração de eventos visível no recorte |
| SI-4 | System Monitoring | Monitoramento SSH evidenciado; cobertura limitada |
| SI-4(5) | System-Generated Alerts | Alerta automático evidenciado por E35 |
| IR-4 | Incident Handling | Análise documentada; ciclo completo de resposta não executado |

`MaxAuthTries` é um limite por conexão, conforme o [manual oficial OpenSSH](https://man.openbsd.org/sshd_config#MaxAuthTries). Sua presença não prova aplicação de uma política de tentativas consecutivas entre conexões.

## 4. CIS Controls v8.1

Versão adotada: [CIS Controls v8.1](https://www.cisecurity.org/controls/v8-1). IDs e títulos conferidos na especificação oficial CIS, com links por família.

| Salvaguarda | Título oficial | Relação com o caso |
|---|---|---|
| 4.1 | Establish and Maintain a Secure Configuration Process | Processo completo não demonstrado |
| 4.4 | Implement and Manage a Firewall on Servers | UFW inativo; outra filtragem não examinada |
| 5.2 | Use Unique Passwords | Não avaliada: senhas válidas e política não foram auditadas |
| 6.3 | Require MFA for Externally-Exposed Applications | Recomendação condicional ao cenário exposto |
| 6.4 | Require MFA for Remote Network Access | Aplicabilidade futura; implementação não demonstrada |
| 8.2 | Collect Audit Logs | Coleta SSH demonstrada no recorte |
| 8.9 | Centralize Audit Logs | Parcial: alertas centralizados; completude não demonstrada |
| 8.10 | Retain Audit Logs | Período e suficiência não demonstrados |
| 8.11 | Conduct Audit Log Reviews | Revisão pontual; periodicidade não demonstrada |
| 13.1 | Centralize Security Event Alerting | Demonstrado para o endpoint do exercício |
| 13.2 | Deploy a Host-Based Intrusion Detection Solution | Agente e detecção demonstrados no recorte |
| 13.7 | Deploy a Host-Based Intrusion Prevention Solution | Prevenção não demonstrada |
| 13.11 | Tune Security Event Alerting Thresholds | Não demonstrada: observar uma regra não comprova ajuste periódico |

Fontes CIS: [Control 4](https://cas.docs.cisecurity.org/en/latest/source/Controls4/), [Control 5](https://cas.docs.cisecurity.org/en/latest/source/Controls5/), [Control 6](https://cas.docs.cisecurity.org/en/latest/source/Controls6/), [Control 8](https://cas.docs.cisecurity.org/en/latest/source/Controls8/) e [Control 13](https://cas.docs.cisecurity.org/en/latest/source/Controls13/).

O título de 13.7 não torna Fail2ban equivalente a uma solução completa de prevenção. Ter Wazuh instalado também não satisfaz automaticamente toda uma salvaguarda.

## 5. Escopo mantido

ISO/IEC 27001 não foi acrescentada à matriz: o escopo original concentra-se em MITRE/NIST/CIS, e não houve análise do texto normativo ISO neste trabalho. Não se deve anunciar conformidade ISO com base neste laboratório.

Evidências e interpretações detalhadas: [relatório SOC](incident-report.md), [riscos](fase11-grc-risco-controles.md) e [índice](evidence-index.md).
