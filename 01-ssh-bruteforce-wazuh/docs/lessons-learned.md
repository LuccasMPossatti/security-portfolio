# Revisão técnica e aprendizados

**Revisão:** 06/11/2026 · **Método:** comparação dos documentos com as imagens disponíveis e consulta a fontes oficiais

Esta é uma revisão documental assistida, não uma auditoria independente formal, pentest ou nova execução do laboratório.

## 1. Achados corrigidos

| Achado | Correção aplicada | Por que importa |
|---|---|---|
| README público descrevia duas VMs | Arquitetura com três VMs | Evita divergência com a execução |
| Link público apontava para pasta inexistente | Entregáveis organizados em `01-ssh-bruteforce-wazuh/` | O leitor passa a encontrar o estudo |
| README prometia JSON/runbook não localizados | Entregas limitadas aos arquivos existentes e ao roteiro preparado | Não apresentar artefatos planejados como entregues |
| E34 descrita como consulta de oito falhas 5760 | Registrada como tela ampla de 18 resultados | Nome do arquivo não substitui leitura do conteúdo |
| Oito hits 5760 confundidos com oito tentativas do ataque | SUP reconciliada: sete do ataque + uma anterior; oitava falha em 5763 | Contar por janela e conteúdo, não por total de busca |
| Sub-técnica dita nunca declarada pelo SIEM | T1110.001 visível em 5760; T1110 em 5763 | Diferenciar metadados dos alertas |
| Data da Janela A considerada desconhecida | E30 mostra 27/09/2026 | A comparação foi coletada depois da Janela B |
| E32/E20 tratadas como arquivos disponíveis | Ausência explícita; uso de fontes disponíveis | Rastreabilidade sem fabricar evidência |
| Precisão de microssegundos atribuída a E32 | Timeline em segundos, apoiada em E26/E35/E24 | Precisão deve corresponder à fonte acessível |
| Ausência de comprometimento afirmada amplamente | Sucesso não observado nos trechos; comprometimento global indeterminado | Evita conclusão maior que o alcance da análise |
| Recortes de autenticação/sessões tratados como período completo de investigação | Limite explícito: E24/E28 não documentam início/fim absolutos de busca abrangente nem instante de captura | Não inventar uma janela de ausência de atividade |
| Exercício controlado confundido com descoberta de ameaça desconhecida | Resumo delimita detecção de estímulo conhecido, com senhas declaradas inválidas | Não generalizar uma execução para eficácia contra ataques desconhecidos |
| Alerta reduzindo numericamente o impacto | Redução de R1b de 16 para 12 retirada por falta de demonstração | Detecção sozinha não comprova redução de dano |
| Ausência de índice tratada como ausência de retenção | E31 descrita como ausência de padrão na interface | Distinguir coleta, arquivo, indexação e busca |
| Host-Only descrito como isolamento absoluto | Host-Only + NAT; host permanece no alcance | Precisão sobre limites da rede |
| Topologia restrita tratada como premissa comprovada de R1a | P=2 explicitamente condicional ao alcance de entrada; ausência de encaminhamento nas configurações salvas não prova o estado histórico | Separar configuração posterior, alcance efetivo e hipótese de risco |
| Referências DET0463/DET0551 sem validação de implementação completa | Retiradas do mapeamento do caso | Não ampliar os resultados além do que foi analisado |
| Histórico de correções repetido nos documentos principais | Retrospectiva concentrada neste documento; SOC/GRC/mapeamentos descrevem o estado atual da análise | Facilitar a leitura sem perder rastreabilidade |
| Hashes descritos como cobertura de todos os textos públicos | Escopo limitado aos oito Markdown de docs/; manifesto sem assinatura, mantido no mesmo repositório | Distinguir comparação de conteúdo de autenticação e proteção contra troca conjunta |
| E33 usada como prova de soft lockup | Conteúdo incompatível registrado; causa permanece não comprovada | Não confiar apenas no nome da imagem |

## 2. O que o projeto demonstra

- Leitura de eventos Linux e distinção entre falha de senha, autenticação aceita e sessão.
- Reconciliação de alertas individuais com uma correlação no SIEM.
- Uso do contexto operacional sem atribuí-lo indevidamente aos logs.
- Registro das limitações de uma investigação baseada em recortes.
- Tradução de achados técnicos em cenários de risco e tratamentos propostos.

## 3. Melhorias futuras, fora dos resultados desta execução

| Melhoria | Critério para afirmar conclusão |
|---|---|
| Recuperar E32 e exportar fontes brutas | Data/origem conferidas, arquivo íntegro e vínculo com a timeline |
| Testar cobertura de correlação | Casos abaixo, próximos e acima da janela/contagem, em ambiente autorizado |
| Exercitar contenção | Ação real, reversão e impacto registrados |
| Avaliar retenção e saúde | Configuração, período, armazenamento e eventos de teste verificados |
| Revisar permissões Linux | Listagens interpretadas, incluindo arquivos e diretórios world-writable |
| Formalizar procedimento de resposta | Exercício de mesa com critérios de escalonamento e responsáveis |

Não se afirma que ajustes de regra, MFA ou bloqueios foram implementados. O exercício atual termina na detecção, investigação e análise documental.

## 4. Explicação curta do caso

“Montei um laboratório com três VMs para acompanhar tentativas de senha SSH. Comparei os registros do endpoint com os alertas Wazuh e confirmei uma correlação de oito falhas pela regra 5763. Depois documentei os limites da investigação e avaliei os riscos e os controles. O teste demonstrou detecção; não demonstrou que um ataque foi bloqueado.”

Para apresentar o projeto, o autor deve conseguir explicar a diferença entre evento e alerta, a composição sete + um, o papel de cada VM, a diferença entre T1110 e T1110.001 e por que as recomendações não são resultados implementados.
