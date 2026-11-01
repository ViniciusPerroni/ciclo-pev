# Plano NN — <Tema>

🟡 Aguardando aprovação · Entrada: `.planning/PROJETO.md` (linha NN) · História: `.planning/historias/<slug>.md` (ou "sem história") · Regras: `CLAUDE.md`

<!-- Estados do cabeçalho: 🟡 Aguardando aprovação → 🔄 Em execução (branch `planoNN/<slug>`) → ✅ Concluído em AAAA-MM-DD (branch `planoNN/<slug>`; N testes) -->

## Objetivo

Ao fim deste plano, <quem> **<faz o quê>**:
- <capacidade 1, em linguagem de usuário>;
- <capacidade 2>.

<Perfis novos, módulos que abrem para quem, o que a documentação ganha.>

## Ponto de partida

### O que o sistema já tem

<O que o módulo reaproveita, respeita ou muda no sistema atual.>

| O que existe | Onde | Uso neste plano |
|---|---|---|
| <entidade, caso de uso, regra de acesso, aviso, relatório> | `caminho/Arquivo:linha` · Plano NN (D-xx) | <reaproveita / respeita a regra / muda (D-NNN)> |

### O legado

<"Busca em `base-legado/`: nenhuma entidade, migration ou tela de X. As regras são funcionalidade nova." Ou a tabela abaixo, se houver resto do tema. Sem legado no projeto: "Não se aplica".>

| Comportamento | Evidência | No novo sistema |
|---|---|---|
| <o que o legado tem> | `caminho/Arquivo:linha` | <mantido / corrigido / muda (D-NNN)> |

### Referências externas

<Normas, legislação ou práticas consultadas e o que delas virou decisão aprovada. "Nenhuma", se não se aplica.>

## Decisões

| # | Decisão | Fonte |
|---|---|---|
| D-NNN | **<Título curto>.** <A regra, com um exemplo concreto de números ou datas quando houver cálculo.> | Resposta do usuário (AAAA-MM-DD) |

## Arquitetura

<Como o módulo se encaixa nas camadas do projeto (postulado de arquitetura do CLAUDE.md).>
- <Domínio: entidades, regras, o que não pode ser apagado>.
- <Dados: configurações, repositórios, migration, índices únicos>.
- <Casos de uso / serviços>.
- <Interface e injeção de dependências>.

## Dados existentes e compatibilidade

<!-- Para evolução de módulo ou acabamento. Em módulo novo: "Não se aplica: o módulo é novo." -->

- **Registros já gravados:** <o que acontece com eles: valor padrão, recálculo, carga ou nada; a migration que cuida disso e o teste>.
- **Telas, regras e perfis que mudam:** <lista, cada uma com a etapa e o artigo da documentação>.
- **Compatibilidade:** <o que continua funcionando igual para o usuário, e o que muda>.

## Referências de interface

<!-- Só em plano com telas. Componente que não existe no projeto é pergunta ao usuário, não decisão. -->

| Tela | Tela-modelo | O que é aproveitado |
|---|---|---|
| <Tela> | <página ou componente de referência> | <componentes e padrões; lista paginada ou simples> |

## Documentação

<!-- Só se o projeto tem documentação de usuário. A documentação de uma tela entra na mesma etapa da tela. -->

| Artigo | Novo ou alterado | O que muda | Etapa |
|---|---|---|---|
| <Artigo> | Novo | <seções, perguntas frequentes> | <N> |

## Fora do escopo

- <O que não entra e, se útil, por quê ou em que plano>.

## Etapas

<!-- Uma etapa por sessão. Tamanho P, M ou G (G é o máximo). Cada etapa deixa o build e os testes verdes. -->

### Etapa 0 — <Título> · M · ⬜ pendente
- **Entregas:** <o que existe ao fim, citando D-NNN (e H-n, CA-n, se houver história)>.
- **Testes:** <casos concretos: exemplos das decisões, outro cliente, perfil barrado>.
- **Critérios de pronto:** <o que a Validação confere nesta etapa>.
- **Validação:** comando de testes verde; revisão do diff da etapa pelo agente `pev-revisor` (Opus) contra este plano e os postulados; achados corrigidos antes do commit.

### Etapa 1 — <Título> · G · ⬜ pendente
- **Entregas:** …
- **Testes:** …
- **Critérios de pronto:** …
- **Validação:** …

### Etapa N — Dados de teste e fechamento · M · ⬜ pendente
- **Entregas:** dados de teste (seed) pelos métodos do domínio; roadmap; conferência no sistema rodando.
- **Testes:** seed repetível e testada.
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
- Cada critério de aceite marcado ✅ com a evidência ou ❌ com o que falta.
- Conferência no sistema rodando, nos perfis do plano e num perfil que deve ser barrado.
- Decisões provisórias apresentadas ao usuário.
- Cabeçalho `✅ Concluído em AAAA-MM-DD` (branch, número de testes) e roadmap atualizados.

## Critérios de aceite

<!-- Na Validação final, cada item recebe ✅ com a evidência (teste ou conferência) ou ❌ com o que falta. -->

### Do plano

- <Comportamento verificável, em linguagem de usuário, com o exemplo da decisão> (prova: <teste ou conferência no sistema>).

### Base (todo plano)

<!-- Copie a lista de `.planning/criterios-base.md`. O que não se aplica fica com "não se aplica: <motivo>". -->

## Perguntas em aberto

<!-- "Nenhuma." com o resumo das que foram respondidas, ou Q1, Q2… com a proposta. Plano com pergunta aberta não é aprovado. -->

- **Q1.** <Pergunta> Proposta: <a sua recomendação e por quê>.

## Execução

<!-- Preenchida durante a implementação, uma entrada por etapa: o que foi feito, arquivos principais, número de testes, o que a Validação encontrou e corrigiu, conferência no sistema. -->

### Decisões provisórias (para revisar)

<!-- Decisões tomadas durante a execução sem perguntar ao usuário, numeradas 1, 2, 3… e agrupadas por etapa, para o usuário revisar item a item. -->
