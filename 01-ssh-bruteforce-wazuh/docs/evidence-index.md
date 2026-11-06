# Fontes, privacidade e rastreabilidade

**Revisão:** 06/11/2026 · **Edição pública anonimizada**

O acervo original recebido reúne **85 arquivos: oito documentos Markdown e 77 imagens (8 + 77 = 85)**. A revisão também recebeu novas capturas de verificação operacional, adicionais a esse total original. Os originais foram preservados privadamente. Esta publicação contém apenas extratos analíticos anonimizados e documentos revisados.

## O que foi preservado nos extratos

Contagens de eventos, horários relevantes, IDs de regras, nível do alerta e a distinção entre observação e interpretação. Endereços do ambiente, nomes de contas, identificadores das máquinas e dados operacionais desnecessários foram omitidos ou substituídos por rótulos.

**ORIGEM-LAB, ALVO-LAB, SIEM-LAB e CONTA-LAB são rótulos de anonimização.** Não representam contas ou endereços utilizáveis.

## Fontes examinadas na revisão privada

| Referência | Conteúdo examinado | Extrato / uso público |
|---|---|---|
| E26 | Eventos anteriores do alerta | Sequência cronológica, em segundos |
| E27 | Timestamp e campos do alerta | Data, ID e nível |
| E35 | Alerta 5763 completo no recorte | Sete anteriores + evento atual |
| SUP | Consulta filtrada 5760 | Sete eventos do incidente + um anterior |
| E34 | Consulta ampla, sem filtro | Correção do total de resultados |
| E36 | Histórico operacional da simulação | Método, sem publicar comandos ou valores de senha |
| E24 / E28 | Trechos de autenticação e sessões | Limites da conclusão de sucesso não observado |
| E29 / E30 | Comparação com autenticações legítimas | Data, contagem e limites da consulta |
| E31 | Padrões disponíveis na interface | Limitação de busca, sem inferir ausência global de retenção |
| E37–E39 | Verificações históricas de controles | Contexto da análise GRC, com ressalva temporal |
| E40 em diante | Revisão operacional de segurança | Arquivo exclusivamente privado |

Leia os [extratos analíticos](evidence-excerpts.md) e o [relatório SOC](incident-report.md).

## Limites da publicação

- Extratos são transcrições analíticas, não exports nativos nem novos eventos.
- A revisão confrontou os documentos com as fontes privadas. Um leitor do repositório não consegue repetir integralmente essa inspeção sem os originais.
- E32 e E20 foram citadas no histórico, mas não localizadas no acervo recebido. O resultado público se apoia nas fontes efetivamente disponíveis.
- Imagens de ensaios anteriores não foram tratadas como prova do incidente em análise.
- A verificação de integridade do pacote público não estabelece cadeia de custódia retroativa do incidente.

## Integridade

[SHA256SUMS.txt](../evidence/SHA256SUMS.txt) lista exclusivamente os **oito arquivos Markdown de `docs/`**. Não cobre os READMEs da raiz e do caso, nem o próprio manifesto. A comparação detecta divergências em relação aos valores de referência, desde que esse manifesto seja considerado confiável.

O manifesto está no mesmo repositório dos documentos e **não é uma assinatura digital**: uma alteração conjunta dos textos e dos hashes pode manter a verificação consistente. Ele não autentica autoria nem oferece, sozinho, garantia contra adulteração. Os hashes dos originais e o inventário com nomes completos ficam no arquivo privado e não são publicados como se fossem prova acessível neste repositório.
