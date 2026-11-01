# Diário do ciclo PEV

Registro vivo do fluxo: o que foi decidido, por quê, o que foi medido e o que deu errado. É a matéria-prima para melhorar o PEV e para explicá-lo a outras pessoas.

**Como usar:** a cada etapa de plano executada, acrescente uma linha na tabela de métricas. Quando algo der errado ou der muito certo, acrescente uma entrada em "Lições". Quando mudar o fluxo, acrescente em "Mudanças no fluxo".

## Métricas por etapa

| Data | Projeto / plano | Etapa | Tam. | Modelo | Contexto (`/clear` antes?) | Tempo | Custo (`/cost`) | Achados do revisor | Retrabalho | Observações |
|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | |

> Colunas: **Tam.** = P, M ou G; **Contexto** = sim/não; **Achados do revisor** = quantos BLOQUEIA/CORRIGIR; **Retrabalho** = o que precisou refazer depois do commit.

## Hipóteses a medir

| # | Hipótese | Como medir | Resultado |
|---|---|---|---|
| H1 | Uma sessão por etapa (com `/clear`) gasta menos tokens e tempo que manter o contexto o plano inteiro, sem perder qualidade | Fazer etapas parecidas dos dois jeitos e comparar tempo e `/cost` | |
| H2 | Sonnet implementando e Opus revisando tem a mesma qualidade que Opus em tudo | Comparar achados do revisor e retrabalho entre etapas feitas em cada modelo | |
| H3 | O revisor independente encontra problemas que a implementação deixou passar | Contar achados BLOQUEIA/CORRIGIR por etapa | |
| H4 | Com história como entrada, o planejamento faz menos perguntas e o plano sai mais fiel ao pedido | Comparar nº de perguntas e de ajustes pedidos, com e sem história | |
| H5 | A matriz de cobertura evita perder regras do legado na migração | Contar regras do legado descobertas só depois do plano | |
| H6 | O CLAUDE.md enxuto (detalhes em `docs/padroes/`) não piora o resultado | Observar se o Sonnet esquece padrões de tela/arquitetura | |

## Lições

<!-- Formato: data · contexto · o que aconteceu · o que mudou ou o que aprendemos -->

- **2026-11-01 · Opus lento na implementação.** Usar Opus para tudo em uma única sessão longa deixava cada resposta lenta e cara. Mudança: `opusplan` (Opus planeja, Sonnet implementa) e uma sessão por etapa.
- **2026-11-01 · O plano precisa ser a memória.** Com uma sessão por etapa, tudo o que a implementação precisa tem de estar no plano (decisões, exemplos, critérios). Mudança: status por etapa no título, seções Execução e Decisões provisórias, regra de ler o plano inteiro antes de implementar.
- **2026-11-01 · Revisão por etapa não vê o conjunto.** A revisão do diff de cada etapa não pega o que atravessa etapas. Mudança: etapa fixa **Validação final** com revisão da branch inteira e critérios de aceite marcados com evidência.
- **2026-11-01 · A documentação envelhece durante o plano.** O texto escrito na etapa da tela fica desatualizado quando etapas seguintes mudam o comportamento. Mudança: etapa fixa **Revisão da documentação**, penúltima.
- **2026-11-01 · Regra genérica no CLAUDE.md não vira tarefa.** A regra "toda mudança vai para a Ajuda" já existia, mas passava despercebida na implementação. Mudança: as skills transformam as regras em etapas, entregas e critérios dentro do plano.
- **2026-11-01 · CLAUDE.md grande custa em toda mensagem.** Ele vai inteiro em cada mensagem de cada sessão. Mudança: de ~15.900 para ~9.100 caracteres, com os detalhes de tela movidos para `docs/padroes/telas.md`, lido sob demanda.
- **2026-11-01 · Módulos novos pedem entrevista; migração pede levantamento.** Num módulo novo as regras vêm do usuário; numa migração vêm do código legado. Mudança: duas skills de planejamento (`pev-planejar` e `pev-planejar-migracao`), esta última com matriz de cobertura e carga de dados.
- **2026-11-01 · Histórias de usuário como entrada.** Escrever o pedido como história (como feito no Financeiro e no CRM) deixa o planejamento mais rápido, mas a história pode estar errada sobre o que já existe. Mudança: `pev-planejar` aceita história, confere no código e pergunta só os pontos nebulosos.
- **2026-11-01 · Nome da skill.** O prefixo `dne-` (DotNet Expert) prendia o fluxo a uma tecnologia. Mudança: prefixo `pev-` (Planejar · Executar · Validar).

## Mudanças no fluxo

| Data | Mudança | Motivo |
|---|---|---|
| 2026-11-01 | `opusplan` como modelo padrão | Opus planeja, Sonnet implementa |
| 2026-11-01 | Postulado P5 e skills `planejar` / `executar` | Fluxo em etapas, uma por sessão |
| 2026-11-01 | Agente `pev-revisor` (Opus) | Revisão independente por etapa |
| 2026-11-01 | Etapas fixas: Revisão da documentação e Validação final | Pegar o que atravessa etapas |
| 2026-11-01 | Critérios de aceite base (`criterios-base.md`) | Todo plano carrega os mesmos critérios |
| 2026-11-01 | `pev-planejar-migracao` | Migração parte do legado, com matriz de cobertura |
| 2026-11-01 | Modelo de história de usuário | Entrada opcional do planejamento |
| 2026-11-01 | Kit extraído para `ciclo-pev` | Reaproveitar em outros projetos |

## Perguntas em aberto sobre o fluxo

- Qual o tamanho ideal de etapa? (P/M/G é suficiente, ou é preciso um limite de arquivos?)
- O revisor precisa de checklists por tipo de etapa (domínio, telas, relatórios)?
- Vale uma skill para "ajuste pequeno fora de plano" (hoje o fluxo não cobre)?
- Como adaptar o `pev-mapear-legado` para outras tecnologias?
