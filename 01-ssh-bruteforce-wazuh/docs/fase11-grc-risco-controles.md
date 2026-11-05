# GRC — Riscos, controles e tratamento

**Autor:** Luccas M. Possatti · **Revisão:** 05/11/2026 · **Escopo:** laboratório SSH/Wazuh

**[O]** observado · **[I]** análise · **[R]** proposta ainda não implementada. As classificações são julgamentos didáticos, não estimativas estatísticas ou decisões formais de aceitação de risco.

## 1. Contexto e ativos

O estudo avalia o acesso SSH à VM-02 e a visibilidade do Wazuh na VM-03. Inclui a conta de laboratório, os eventos de autenticação e a disponibilidade da coleta. Não há dados corporativos, valores de perda ou métricas de exposição de produção no escopo.

A rede do exercício foi documentada como Host-Only. A inspeção posterior das configurações salvas encontrou também NAT nas três VMs. Host-Only inclui o host e outras VMs conectadas; não significa isolamento absoluto. A configuração atual não comprova a configuração exata na data do incidente.

## 2. Controles e limites da evidência

| Observação | Evidência | Limite |
|---|---|---|
| `PasswordAuthentication yes` e `PubkeyAuthentication yes` | E37 | Aceitar chave não prova que o usuário tenha chave configurada; não comprova ausência de outros métodos |
| `PermitRootLogin without-password` | E37 | Root por senha é restringido; não significa root totalmente desabilitado |
| `MaxAuthTries 6` e `LoginGraceTime 120` | E37 | Limite por conexão e tempo de autenticação; não equivalem a bloqueio entre conexões |
| UFW e Fail2ban `inactive` | E37 | Não comprova ausência de nftables/iptables, outros serviços ou mecanismos preventivos |
| Bloco `<active-response>` comentado no `ossec.conf` examinado | E38/E39 | Configuração centralizada, includes e agentes não foram inspecionados integralmente |
| Falhas correlacionadas pela regra 5763 | E35/E27 | Detecção demonstrada; nenhum bloqueio documentado |
| `wazuh-archives-*` não aparece entre os padrões disponíveis | E31 | Não comprova ausência global de arquivos de archives ou de retenção |

As configurações E37–E39 são posteriores ao incidente. O experimento usou valores de senha declarados inválidos: a probabilidade de adivinhar a credencial real **não foi medida**.

## 3. Método 5×5

Pontuação = probabilidade × impacto. Escalas e faixas são escolhas deste exercício, sem alegação de que constituam uma matriz prescrita pelo NIST ou CIS.

| Nota | Probabilidade qualitativa | Impacto no escopo |
|---|---|---|
| 1 | Rara sob premissas fortes de restrição | Efeito desprezível e reversível |
| 2 | Pouco provável no contexto restrito | Degradação limitada da análise/serviço |
| 3 | Plausível, com incerteza material | Acesso indevido ou perda relevante no ativo do laboratório |
| 4 | Elevada sob premissas do cenário | Comprometimento relevante e alcance ampliado hipotético |
| 5 | Muito elevada sob as premissas | Impacto severo e amplo, não demonstrado neste laboratório |

| P / I | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| 5 | 5 | 10 | 15 | 20 | 25 |
| 4 | 4 | 8 | 12 | 16 | 20 |
| 3 | 3 | 6 | 9 | 12 | 15 |
| 2 | 2 | 4 | 6 | 8 | 10 |
| 1 | 1 | 2 | 3 | 4 | 5 |

Faixas: **1–4 Baixo; 5–9 Médio; 10–16 Alto; 17–25 Crítico**. Valores ordinais ordenam a discussão; não permitem calcular percentuais de redução de risco.

## 4. Registro de riscos

| ID | Evento e consequência | P × I | Avaliação contextual [I] | Premissas e incertezas |
|---|---|---|---|---|
| R1a | Credencial adivinhada e acesso SSH indevido à VM-02 no laboratório | 2 × 3 = **6 Médio** | Nota de trabalho, condicional | Exposição limitada à rede do laboratório; sem dados de valor empresarial. Robustez da senha e privilégios efetivos desconhecidos. A nota não decorre das oito falhas. |
| R1b | Mesmo evento em servidor hipotético exposto a rede não confiável | 4 × 4 = **16 Alto** | Cenário hipotético para priorização | Assume senha suscetível, serviço alcançável e acesso a ativos relevantes. Exposição não foi observada neste laboratório. Sem essas premissas, a nota precisa ser refeita. |
| R2 | Tentativas lentas ou distribuídas não geram a correlação esperada, atrasando a triagem | 3 × 2 = **6 Médio** | Hipótese de cobertura | Não houve teste fora da sequência observada. Falhas individuais ainda podem gerar alertas; não é perda total de visibilidade. |
| R3 | Indisponibilidade da coleta ou retenção insuficiente prejudica uma investigação | 3 × 2 = **6 Médio** | Cenário de continuidade | Histórico relata travamentos; perda real não foi medida. Ausência do padrão archives não prova ausência de retenção. |

P3 em R2/R3 representa uma hipótese conservadora para discussão, não a frequência observada. A avaliação não fixa um horizonte temporal nem dispõe de população de eventos; não deve ser apresentada como previsão de perdas ou chance de ataque.

**Sensibilidade:** R1a passaria de 6 para 9 se a probabilidade assumida subisse de 2 para 3. R1b varia de 12 a 20 com P entre 3 e 5 e I=4. O desconhecimento da credencial real e do contexto impede tratar uma nota isolada como resultado medido.

## 5. Inerente, atual e residual

Um risco inerente exige um cenário de referência sem os controles selecionados. Esse cenário não foi executado; a topologia restrita já é parte do contexto atual. Portanto, não se apresenta um valor inerente como se tivesse sido medido.

As notas da seção 4 são **avaliações contextuais atuais**, antes dos tratamentos propostos. Detecção pode apoiar uma resposta, mas não foi medido tempo de resposta nem houve exercício de contenção. Não há base para reduzir o impacto de 4 para 3 somente porque um alerta existe. A redução anterior de R1b de 16 para 12 foi retirada.

Nenhuma redução numérica de risco residual é declarada alcançada. Após tratamento e teste, as notas deverão ser reavaliadas com as mesmas premissas e escalas.

## 6. Plano de tratamento [R]

| Risco | Tratamento proposto | Prioridade | Critério de validação futuro |
|---|---|---|---|
| R1a/R1b | Configurar chave e validar acesso antes de restringir autenticação por senha; revisar também keyboard-interactive/PAM | Alta | Acesso autorizado por chave funciona; senha é recusada conforme a política, com acesso de recuperação disponível |
| R1a/R1b | Restringir origens e revisar alcance de rede | Alta | Origem autorizada alcança o serviço; origem não autorizada é recusada em teste controlado |
| R1a/R1b | Avaliar MFA conforme o acesso remoto e o contexto | Média | Fluxos e exceções documentados e testados; não confundir MFA com chave pública isolada |
| R1a/R1b | Avaliar bloqueio temporário por origem e risco de negação de serviço | Média | Ação, duração, exceções, reversão e logs demonstrados |
| R2 | Testar variações de cadência, múltiplas origens e fronteiras da correlação | Média | Matriz de testes com resultado esperado/observado e sem promessa de cobertura universal |
| R3 | Conferir coleta, retenção, armazenamento e monitoramento de saúde | Média | Eventos de teste preservados e pesquisáveis; política e alertas de indisponibilidade definidos |
| R1–R3 | Definir procedimento de triagem e escalonamento | Média | Exercício de mesa registra decisões, responsáveis e evidências |

**Responsável pela validação:** operador do laboratório. **Prazo:** a definir em uma rodada de melhorias; não há compromisso de implementação nesta versão.

Aceitar risco é uma decisão do responsável, com escopo e duração. A revisão não registra aceitação em nome do autor e não recomenda transportar a configuração didática para produção.

## 7. Relação com a apresentação profissional

É defensável afirmar: “Transformei um exercício de detecção em registro de riscos e plano de tratamento, distinguindo controles demonstrados de recomendações.” Não é defensável afirmar redução percentual de risco, conformidade com frameworks, prevenção de intrusão ou operação de SOC de produção a partir deste teste.

Veja [mapeamento](control-mapping.md), [relatório SOC](incident-report.md) e [índice de evidências](evidence-index.md).
