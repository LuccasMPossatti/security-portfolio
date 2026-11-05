# Segurança operacional e privacidade

**Revisão:** 05/11/2026

## Escopo da conferência

O fechamento do laboratório inclui uma verificação de permissões, serviços em escuta, filtragem e alcance das redes virtuais. Os resultados detalhados, imagens e configurações atuais são mantidos em arquivo privado, separados da edição pública.

Essas consultas não alteram permissões, autenticação ou firewall. Servem para reconhecer a postura do ambiente e registrar limites; não constituem uma auditoria completa de segurança do computador ou da rede doméstica.

## Pontos de aprendizado

- Um serviço escutando em todas as interfaces não comprova exposição à Internet: é necessário verificar o caminho de rede.
- Host-Only inclui comunicação com o host e outras VMs da rede. NAT permite saída e deve ser avaliado em conjunto com encaminhamentos, roteamento e controles do host.
- UFW inativo não prova ausência de outras regras de filtragem.
- Arquivo comum world-writable e diretório temporário com sticky bit exigem interpretações diferentes.
- Uma consulta sem resultado precisa ser interpretada considerando privilégio, filesystem, erros e cobertura.
- Permissões observadas hoje não demonstram que determinado comando nunca tenha sido usado no passado.
- Encerrar as VMs ao terminar o trabalho é parte do procedimento operacional do laboratório.

Referência de rede: [Oracle VirtualBox — Virtual Networking](https://docs.oracle.com/en/virtualization/virtualbox/7.2/user/networkingdetails.html).

## Política desta publicação

Não são distribuídos discos, configurações operacionais atuais, credenciais, usuários locais, endereços do ambiente, screenshots originais, logs brutos ou resultados detalhados da auditoria. Os rótulos usados nos documentos são anonimizados.

Os [extratos analíticos](evidence-excerpts.md) preservam o resultado técnico e deixam explícito o que foi transcrito e o que não está disponível ao leitor. A [análise de riscos](fase11-grc-risco-controles.md) descreve os limites históricos do experimento, sem divulgar um inventário atual de exposição.
