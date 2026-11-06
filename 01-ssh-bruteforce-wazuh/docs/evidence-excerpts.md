# Extratos analíticos das evidências

**Edição pública anonimizada — revisão em 06/11/2026**

As tabelas abaixo foram transcritas e normalizadas a partir dos registros examinados na revisão privada. Não são logs brutos, exports JSON ou screenshots originais. Identificadores foram substituídos por rótulos. Não foram inventados novos eventos para preencher lacunas.

## E35 — Correlação de falhas

| Campo | Valor relevante |
|---|---|
| Data do evento | 10/09/2026 |
| Timestamp do alerta | 20:27:21.468 UTC |
| Regra | 5763 |
| Nível | 10 |
| frequency | 8 |
| Mapeamento do alerta | T1110 |
| Origem / alvo / conta | ORIGEM-LAB / ALVO-LAB / CONTA-LAB |
| Composição visível | Sete falhas anteriores em previous_output + uma falha no evento atual |

Essa composição sustenta oito falhas correlacionadas. O valor do campo `frequency`, isoladamente, não substitui a leitura dos eventos.

## E26 e E24 — Sequência temporal

| Evento | UTC |
|---|---|
| Falha 1 | 20:26:57 |
| Falha 2 | 20:27:01 |
| Falha 3 | 20:27:03 |
| Falha 4 | 20:27:07 |
| Falha 5 | 20:27:09 |
| Falha 6 | 20:27:13 |
| Falha 7 | 20:27:16 |
| Falha 8 | 20:27:20 |

Os sete primeiros horários estão no conjunto anterior da correlação; a última falha também aparece no trecho de autenticação E24, com data completa. A publicação usa precisão em segundos, adequada às fontes disponíveis. O intervalo é de aproximadamente 23 s.

## SUP e E34 — Resultados de busca não são tentativas

| Consulta examinada | Resultado | Reconciliação |
|---|---|---|
| SUP, regra 5760 filtrada pela origem | Oito resultados | Sete do incidente; um anterior |
| E34, consulta ampla sem filtro | Dezoito resultados | Inclui tipos diferentes de eventos; não equivale a dezoito tentativas |

Os alertas individuais 5760 exibem T1110.001; a correlação 5763 exibe T1110. Não se atribui automaticamente o conjunto de metadados de uma regra à outra.

## E36 — Método da simulação

O histórico examinado contém uma sequência de oito invocações de Hydra, com uma senha individual por invocação, separadas por ponto e vírgula. O operador declarou valores inválidos. Não houve wordlist nessa sequência. `-t 1` limita tarefas dentro de cada execução.

O histórico também contém uma tentativa avulsa anterior e não exibe timestamps de execução. Portanto, não deve ser usado sozinho para contar o incidente nem para provar sua data. A atribuição de ferramenta depende do contexto operacional, não do log SSH.

## E24 e E28 — Investigação

O trecho de autenticação e o recorte de sessões examinados não exibem sucesso correspondente à sequência. As consultas têm limites de recorte. Não sustentam ausência global de comprometimento, persistência ou atividade em outras fontes.

E24 contém as últimas vinte linhas que correspondem ao filtro textual; E28 contém as dez primeiras linhas da consulta de sessões. Não há início/fim absolutos documentados para uma investigação abrangente, nem horário de captura explicitamente registrado nessas imagens. Não se deduz um período completo de ausência de atividade a partir dos horários das linhas exibidas.

## E29 e E30 — Comparação legítima

E30 mostra cinco autenticações aceitas em 27/09/2026, posteriores ao evento de 10/09. E29 mostra uma busca 5763 sem resultados em janela relativa de 48 horas, sem limites absolutos visíveis. Essa comparação não mede taxa de falsos positivos.

## E37–E39 — Relação com os riscos

As verificações históricas documentaram autenticação por senha permitida, ausência de uso demonstrado dos mecanismos de bloqueio avaliados e um bloco de Active Response comentado no arquivo examinado. Foram feitas depois do incidente, com escopo de configuração limitado. Elas apoiam a discussão de lacunas; não comprovam retrospectivamente toda a configuração do evento.

Veja o [registro de riscos](fase11-grc-risco-controles.md) e os [limites das fontes](evidence-index.md).
