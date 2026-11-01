# Plano NN — Migração de <Módulo>

🟡 Aguardando aprovação · Entrada: `.planning/PROJETO.md` (linha NN), `.planning/migracao/*` · Regras: `CLAUDE.md`

<!-- Estados do cabeçalho: 🟡 Aguardando aprovação → 🔄 Em execução (branch `planoNN/<slug>`) → ✅ Concluído em AAAA-MM-DD (branch `planoNN/<slug>`; N testes) -->

## Objetivo

Ao fim deste plano, o módulo <Módulo> do legado funciona no sistema novo, com <as mudanças principais>:
- <capacidade 1, em linguagem de usuário>;
- <capacidade 2>;
- os dados do legado carregados no novo schema.

<Perfis que usam, o que a documentação ganha.>

## O que o legado faz

<Uma frase de resumo do módulo no legado. Controllers, entidades e relatórios que o compõem (do INVENTARIO.md).>

| Comportamento | Evidência | No novo sistema |
|---|---|---|
| <regra, campo ou comportamento> | `base-legado/.../Arquivo:linha` | <mantido / muda (D-NNN) / não migra (D-NNN) / defeito corrigido (D-NNN)> |

### Regras implícitas e defeitos

| Item | Evidência | Decisão |
|---|---|---|
| <regra implícita (I-n) ou defeito> | `base-legado/.../Arquivo:linha` · `DOMINIO.md` (I-n) ou `QUALIDADE.md` (D-n) | <ainda vale / corrige / descarta> (D-NNN) |

## Matriz de cobertura

<!-- Lista exaustiva do que o legado tem no módulo. Nenhum item sem destino. Na Validação final, o pev-revisor confere cada linha. -->

| Item do legado | Tipo | Destino | Decisão | Etapa |
|---|---|---|---|---|
| Rota/ação do legado (ex.: `Controller.acao`), tela, relatório, job, regra | Ponto de entrada / Tela / Relatório / Job / Regra | Migra igual / Migra com mudança / Não migra / Defeito corrigido | D-NNN | <N> |

## Dependências do sistema novo

<O que os planos anteriores já entregaram e este plano usa.>

| O que existe | Onde | Uso neste plano |
|---|---|---|
| <entidade, regra de acesso, perfil, aviso> | `caminho/Arquivo:linha` · Plano NN (D-xx) | <reaproveita / respeita a regra / estende (D-NNN)> |

## Decisões

| # | Decisão | Fonte |
|---|---|---|
| D-NNN | **<Título curto>.** <A regra, com um exemplo concreto. Divergência do legado: "Diverge do legado, onde …" e o motivo.> | Legado (`caminho:linha`) / Resposta do usuário (AAAA-MM-DD) |

## Carga de dados do legado

<!-- Os dados do legado são migrados por script. Dado sensível respeita o isolamento definido nos postulados. -->

| Tabela do legado | Tabela nova | Colunas e conversões | Tratamento de dados inconsistentes |
|---|---|---|---|
| `<Tabela>` | `<Tabela>` | <coluna → coluna; tipo A → tipo B; enum X → enum Y> | <nulos, órfãos, duplicados: o que fazer> |

- **Volume:** <linhas por tabela, se conhecido, ou "não medido">.
- **Verificação:** <como se confere que a carga está certa: contagens, somas, amostras>.
- **Reexecução:** <o script é repetível? como desfazer?>.

## Arquitetura

<Como o módulo se encaixa nas camadas do projeto (postulado de arquitetura do CLAUDE.md), sem copiar a arquitetura do legado.>
- <Domínio: entidades, regras, o que não pode ser apagado>.
- <Dados: configurações, repositórios, migration, índices únicos>.
- <Casos de uso / serviços>.
- <Interface e injeção de dependências>.

## Referências de interface

<!-- O legado define o conteúdo (campos, ações, fluxo); a tela-modelo do projeto define o visual. Componente que não existe no projeto é pergunta ao usuário. -->

| Tela | Conteúdo no legado | Tela-modelo | O que é aproveitado |
|---|---|---|---|
| <Tela> | `base-legado/.../Tela` (campos e ações) | <página ou componente de referência> | <componentes e padrões; lista paginada ou simples> |

## Documentação

<!-- Só se o projeto tem documentação de usuário. A documentação de uma tela entra na mesma etapa da tela. -->

| Artigo | Novo ou alterado | O que muda | Etapa |
|---|---|---|---|
| <Artigo> | Novo | <seções, perguntas frequentes> | <N> |

## Fora do escopo

- <O que não migra nesta versão e, se útil, por quê ou em que plano>.

## Etapas

<!-- Uma etapa por sessão. Tamanho P, M ou G (G é o máximo). Cada etapa deixa o build e os testes verdes. -->

### Etapa 0 — <Título> · M · ⬜ pendente
- **Entregas:** <o que existe ao fim, citando D-NNN>.
- **Testes:** <para cada regra "migra igual", o caso do legado; para cada divergência, o comportamento novo; outro cliente, perfil barrado>.
- **Critérios de pronto:** <o que a Validação confere nesta etapa>.
- **Validação:** comando de testes verde; revisão do diff da etapa pelo agente `pev-revisor` (Opus) contra este plano, a matriz de cobertura e os postulados; achados corrigidos antes do commit.

### Etapa 1 — <Título> · G · ⬜ pendente
- **Entregas:** …
- **Testes:** …
- **Critérios de pronto:** …
- **Validação:** …

### Etapa N — Carga de dados e seed · M · ⬜ pendente
- **Entregas:** script de carga do legado (conforme a seção "Carga de dados"); dados de teste (seed) pelos métodos do domínio; roadmap; conferência no sistema rodando.
- **Testes:** a carga sobre uma amostra do legado (contagens e somas batendo); repetibilidade; seed testada.
- **Critérios de pronto:** …
- **Validação:** …

### Etapa N+1 — Revisão da documentação · P · ⬜ pendente
<!-- Fixa em todo plano. A documentação de cada tela já entrou na etapa da tela; aqui ela é conferida contra o sistema como ficou. -->
- **Artigos novos a conferir:** <da seção "Documentação" do plano>.
- **Artigos existentes afetados:** <menu, navegação, perfis, outros módulos que mudaram>.
- **Conferir:** as regras citam as mensagens finais das telas; perguntas frequentes; testes de cobertura da documentação.
- **Validação:** revisão dos artigos contra as telas e as decisões finais (incluindo as provisórias).

### Etapa N+2 — Validação final · P · ⬜ pendente
- Comando de testes verde; migrations aplicando num banco limpo.
- Revisão do diff completo da branch pelo agente `pev-revisor` (Opus) contra o plano inteiro: cada D-NNN implementada, nada fora do escopo, postulados.
- **Matriz de cobertura conferida linha a linha:** todo item "migra" implementado e testado; todo item "não migra" ausente.
- Cada critério de aceite marcado ✅ com a evidência ou ❌ com o que falta.
- Conferência no sistema rodando, nos perfis do plano e num perfil que deve ser barrado.
- Decisões provisórias apresentadas ao usuário.
- Cabeçalho `✅ Concluído em AAAA-MM-DD` (branch, número de testes) e roadmap atualizados.

## Critérios de aceite

<!-- Na Validação final, cada item recebe ✅ com a evidência (teste ou conferência) ou ❌ com o que falta. -->

### Do plano

- <Comportamento verificável, em linguagem de usuário, com o exemplo do legado ou da decisão> (prova: <teste ou conferência no sistema>).

### Migração (todo plano de migração)

- **Paridade:** cada linha da matriz de cobertura está implementada e testada (migra), ou comprovadamente ausente (não migra); nenhum item sem destino.
- **Divergências:** toda diferença em relação ao legado é uma decisão `D-NNN` com o motivo e tem teste do comportamento novo.
- **Carga de dados:** o script carrega a amostra do legado com contagens e somas conferidas, trata os dados inconsistentes da seção "Carga de dados" e pode ser reexecutado.

### Base (todo plano)

<!-- Copie a lista de `.planning/criterios-base.md`. O que não se aplica fica com "não se aplica: <motivo>". -->

## Perguntas em aberto

<!-- "Nenhuma." com o resumo das que foram respondidas, ou Q1, Q2… com a proposta. Plano com pergunta aberta não é aprovado. -->

- **Q1.** <Pergunta, citando o comportamento do legado com a evidência> Proposta: <a sua recomendação e por quê>.

## Execução

<!-- Preenchida durante a implementação, uma entrada por etapa: o que foi feito, arquivos principais, número de testes, o que a Validação encontrou e corrigiu, conferência no sistema. -->

### Decisões provisórias (para revisar)

<!-- Decisões tomadas durante a execução sem perguntar ao usuário, numeradas 1, 2, 3… e agrupadas por etapa, para o usuário revisar item a item. -->
