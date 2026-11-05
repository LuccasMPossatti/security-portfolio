# Relatório SOC — Falhas SSH correlacionadas pelo Wazuh

**Autor:** Luccas M. Possatti · **Evento:** 10/09/2026 · **Revisão:** 05/11/2026

**[O]** observação direta dos arquivos disponíveis na revisão privada · **[I]** interpretação · **[R]** recomendação. Todos os horários abaixo são UTC, salvo indicação explícita. Esta edição usa rótulos anonimizados; os originais não integram o repositório público.

## 1. Resumo executivo

O Wazuh correlacionou oito falhas SSH da origem `ORIGEM-LAB` contra a conta `CONTA-LAB` da VM-02 (`ALVO-LAB`). O alerta 5763, nível 10, tem timestamp `2026-09-10T20:27:21.468+0000` (E27/E35). A sequência contém sete eventos anteriores e a falha atual.

Os trechos de autenticação e sessões examinados não mostram login bem-sucedido correspondente ao evento. Essa conclusão se limita ao conteúdo visível. Não foi feita investigação abrangente de persistência, processos ou integridade do endpoint. O exercício comprovou detecção e análise; não comprovou bloqueio ou contenção.

## 2. Escopo e ambiente

| Máquina | Endereço do exercício | Papel |
|---|---|---|
| VM-01 | ORIGEM-LAB | Origem da simulação com Hydra |
| VM-02 | ALVO-LAB | Alvo OpenSSH e Wazuh Agent |
| VM-03 | SIEM-LAB | Wazuh Manager, Indexer e Dashboard |

E05-13 do agente confirma versão 4.14.7 e estado Active no registro de implantação. Não é uma medição contínua da disponibilidade. A versão do conjunto Wazuh foi registrada como 4.14.7 no histórico; não há exportação de inventário completo dos componentes no pacote.

Os alertas de E35 e SUP indicam `location: journald`. `auth.log` e a coleta Wazuh refletem eventos do mesmo serviço SSH. `last -i` consulta registros `wtmp`, uma fonte de sessões distinta, ainda no mesmo endpoint e sujeita às suas limitações.

## 3. Comparação legítima — Janela A

**[O] E30:** cinco alertas 5715, datados de **27/09/2026**. O campo do evento contém os horários 17:21:59, 17:36:42, 17:51:26, 18:06:12 e 18:21:35 UTC. Os horários da coluna do Dashboard aparecem aproximadamente três horas antes.

**[O] E29:** consulta 5763 sem resultados, com filtro relativo de 48 horas. O print não apresenta os limites absolutos dessa consulta; seu alinhamento exato com E30 não pode ser reconstituído apenas por essa imagem.

**[I]** A comparação comprova exemplos de autenticação aceita. Não mede taxa de falsos positivos nem prova que a regra nunca dispara em atividade legítima. A nomenclatura A/B identifica cenários, não ordem cronológica: o ataque de 10/09 antecede os logins de 27/09.

## 4. Simulação

**[O] E36:** o histórico da VM-01 contém uma sequência de oito comandos Hydra, separados por ponto e vírgula, com uma senha individual por comando (`-p`), o mesmo usuário e alvo, e `-t 1`. Não há wordlist nessa sequência. Também aparece uma tentativa avulsa anterior; não deve ser somada ao incidente sem correlação temporal.

**[I]** O histórico demonstra o método registrado pelo operador; isoladamente, não prova quando cada comando foi executado. A conexão com a Janela B depende de E26/E35/E24 e do contexto operacional documentado.

O operador declarou as senhas deliberadamente inválidas. Portanto, o experimento avalia a detecção de falhas, não a resistência da credencial real. Os logs SSH não identificam o Hydra.

## 5. Detecção e reconciliação

| Fonte | Observação direta | Interpretação permitida |
|---|---|---|
| E35 | Uma ocorrência 5763, nível 10, `frequency=8`; sete falhas no `previous_output` e uma no evento atual | Oito falhas correlacionadas |
| E26 | Texto do `previous_output` extraído de `alerts.json` | Confere os sete horários anteriores, em segundos |
| E27 | Timestamp, origem, agente, ID e nível do alerta | Confirma a emissão às 20:27:21.468 UTC |
| SUP | Consulta `rule.id:5760 AND data.srcip:"ORIGEM-LAB"`: oito resultados | Sete do ataque; um anterior, às 20:02:54 no evento de origem |
| E34 | Consulta sem filtro, 18 resultados, incluindo 5763, 5760 e PAM | Contexto; não comprova oito alertas 5760 |

Não se soma o total de resultados de uma busca ampla como se fosse o número de tentativas do ataque. A composição observada é de sete alertas individuais 5760 e uma falha atual associada ao alerta 5763.

Na versão oficial consultada do ruleset 4.14.7, 5760 é uma falha individual e 5763 usa `if_matched_sid=5760`, `same_source_ip`, `frequency=8`, `timeframe=120` e `ignore=60`. Isso explica a relação entre as regras; não demonstra que toda variação de ritmo produzirá o mesmo resultado. Não foram executados testes sistemáticos de fronteira.

Fonte: [ruleset oficial Wazuh 4.14.7](https://github.com/wazuh/wazuh/blob/v4.14.7/ruleset/rules/0095-sshd_rules.xml).

## 6. Timeline reconstituída

Data: **10/09/2026**. Os sete primeiros horários vêm de E26/E35; o último também está no log com data completa de E24. A tabela usa segundos para não atribuir precisão ausente aos primeiros eventos.

| UTC | Registro |
|---|---|
| 20:26:57 | Falha 1, em previous_output |
| 20:27:01 | Falha 2, em previous_output |
| 20:27:03 | Falha 3, em previous_output |
| 20:27:07 | Falha 4, em previous_output |
| 20:27:09 | Falha 5, em previous_output |
| 20:27:13 | Falha 6, em previous_output |
| 20:27:16 | Falha 7, em previous_output |
| 20:27:20 | Falha 8, evento atual; E24 mostra 20:27:20.150548 |
| 20:27:21.468 | Timestamp do alerta 5763, E27/E35 |

O intervalo visível é de aproximadamente **23 segundos**. A diferença nominal de cerca de 1,32 s entre a última falha e o timestamp do alerta não é MTTD: trata-se de um único evento, envolvendo timestamps de componentes distintos, sem medição do desvio entre relógios durante a coleta.

### Artefato E32 ausente

O histórico e o relatório anterior citam uma E32 de `auth.log.1`, com duração de 22,51 s. Esse arquivo não foi localizado nos 85 arquivos recebidos. Não foi substituído por um print antigo nem reconstituído como se fosse original. A contagem de oito é sustentada pela composição de E35, apoiada por E26/E24; a precisão inicial em microssegundos não é adotada nesta revisão.

E07, E21 e as duas imagens chamadas E23 mostram **31/08/2026**. São registros de outro ensaio e não sustentam a timeline de 10/09.

## 7. Investigação e hipóteses

**[O] E24:** o comando filtra a origem em `auth.log` e exibe as últimas vinte linhas correspondentes. O trecho contém falhas e desconexões em 10/09; não contém `Accepted password`. Há também uma linha posterior de `sudo` cujo comando menciona o IP — ela não representa tráfego SSH.

**[O] E28:** as dez primeiras linhas de `last -i` não mostram sessão SSH correspondente ao evento. Há registros de sessões antigas e de reinicialização. O resultado é evidência de apoio, não um inventário completo de sessões ou métodos de acesso.

| Hipótese | Avaliação e limite |
|---|---|
| Tentativas sequenciais de senha | Sustentadas pelo padrão de falhas, correlação e histórico operacional |
| Erros manuais de usuário | Menos compatíveis com o exercício documentado; somente a cadência não identifica a ferramenta |
| Cliente em repetição automática | Os logs, sem contexto operacional, não excluem essa alternativa |
| Login bem-sucedido durante a sequência | Não observado nos trechos examinados; senhas inválidas foram uma condição declarada do teste |
| Persistência ou atividade posterior | Não avaliada de forma abrangente; ausência de linhas visíveis não permite descartá-la globalmente |

## 8. Conclusão e confiança

| Conclusão | Base | Confiança da revisão |
|---|---|---|
| Oito falhas SSH correlacionadas | E35, E26 e E24 | Alta para o conteúdo visível |
| Regra 5763 / nível 10 | E35 e E27 | Alta |
| Hydra registrado como método do operador | E36 e contexto documentado | Alta para os comandos; sem timestamp no histórico |
| Nenhum sucesso exibido na janela do evento | E24 e E28 | Moderada, limitada ao recorte |
| Ausência global de comprometimento | Fontes insuficientes para essa conclusão | Não determinada |

**Conclusão pública:** o Wazuh detectou e correlacionou oito falhas SSH em um exercício controlado. Não foi observado acesso bem-sucedido nos trechos analisados. O teste não demonstrou prevenção, contenção, ausência global de comprometimento ou resistência da senha real.

## 9. Limitações e encaminhamentos

- Originais preservados em arquivo privado. Esta edição pública apresenta extratos anonimizados, sem screenshots ou exports brutos.
- E32 ausente; a precisão original atribuída à timeline não é reapresentada como validada.
- E31 mostra ausência de um padrão `wazuh-archives-*` na interface. Isso não prova inexistência de arquivos de archive no Manager.
- Travamentos foram relatados no histórico do laboratório, mas sua causa e eventual perda de ingestão não foram medidas. O arquivo chamado E33 é uma tela do Discover, não uma comprovação visual de soft lockup.
- Configurações E37–E39 foram verificadas depois do incidente; não comprovam retrospectivamente a configuração em 10/09.
- Sem testes de variação de ritmo, várias contas, várias origens, sucesso posterior ou desempenho em produção.

Próximos documentos: [riscos e controles](fase11-grc-risco-controles.md), [mapeamento](control-mapping.md) e [índice de evidências](evidence-index.md).
