# Roteiro para uma nova execução controlada

Este documento descreve um plano de reprodução. Não é um registro de nova execução nem uma implantação automatizada. A versão 4.14.7 está associada ao caso histórico; uma instalação futura deve registrar as versões efetivamente utilizadas.

## 1. Pré-requisitos

- Três VMs de laboratório sob controle do operador: origem, alvo OpenSSH com agente e servidor Wazuh.
- Rede e alcance revisados antes do teste. Usar somente os endereços das VMs autorizadas.
- Conta local exclusiva do laboratório, credencial válida preservada fora das evidências públicas e acesso de recuperação disponível.
- Horários UTC e estado de sincronização registrados em cada VM.
- Snapshot ou cópia de recuperação e espaço de armazenamento suficiente, conforme os recursos disponíveis.
- Coleta de eventos SSH confirmada no agente e no Wazuh, com fontes e regras efetivas registradas.

## 2. Coleta antes do teste

Registrar versões, IPs, horário, condição dos serviços, regras 5760/5763 realmente carregadas e a origem de coleta. Capturar um login autorizado para comprovar a coleta de sucesso e encerrar essa sessão. Não misturar a janela desse login com a de falhas.

## 3. Estímulo controlado

No caso histórico, o operador registrou oito invocações sequenciais de Hydra, uma senha inválida por invocação, contra uma única conta de laboratório. `-t 1` limitou tarefas dentro de cada invocação; os separadores de comandos estabeleceram a sequência. Não houve wordlist nessa série.

Uma nova execução deve manter alvo e origem autorizados, registrar o comando usado e a janela exata, e parar ao concluir a sequência planejada. O número de comandos não substitui a contagem dos eventos reais recebidos pelo servidor.

## 4. Conferências

| Pergunta | Evidência a coletar |
|---|---|
| Quantas falhas chegaram ao SSH? | Export do log com data completa, origem, usuário e timestamps |
| Quais alertas foram gerados? | Export JSON com `rule.id`, `agent.id`, `data.srcip`, timestamp e full_log |
| Houve correlação? | Alerta completo, eventos anteriores e evento atual |
| Houve login bem-sucedido? | Busca na janela por sucessos e verificação de sessões, sem usar somente `tail` como prova de ausência global |
| A coleta estava estável? | Estado do agente/Manager, espaço e registros de indisponibilidade |

No Discover, filtrar por agente, origem e intervalo absoluto. Separar 5760 e 5763. Não contar alertas PAM adicionais como novas tentativas de senha e não usar uma busca de 30 minutos como se fosse apenas a sequência do incidente.

## 5. Critérios de aceitação

- Quantidade do log reconciliada com os alertas e a janela do teste.
- Regra efetiva e composição da correlação documentadas.
- Sucessos analisados com o mesmo escopo temporal e de identidade.
- Conclusão restrita ao que foi coletado; valores de precisão, eficácia ou risco sustentados pela fonte.
- Logs brutos preservados de forma privada; cópias públicas revisadas e hashes calculados no momento da coleta.

Quando o resultado divergir do caso histórico, documentar a diferença em vez de editar evidências para fazer a contagem coincidir. Testes de fronteira e contenção são novos experimentos, separados do resultado atual.
