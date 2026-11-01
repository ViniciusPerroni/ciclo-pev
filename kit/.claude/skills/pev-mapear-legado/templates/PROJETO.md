<!--
Guia de preenchimento (apague este comentário no arquivo final):
- Preencha só depois da Fase 4.
- Fontes: respostas do usuário (Decisões confirmadas) e ANALISE-CONSOLIDADA.md.
- Campo sem resposta nem evidência fica "a confirmar". Não invente prazo, motivação nem arquitetura alvo.
- Este é o documento de entrada do plano de migração. Seja curto: detalhes ficam nos documentos de .planning/migracao/.
-->

# {{Nome do projeto}}

Atualizado em {{data}}

## Visão

{{1 a 3 frases: o que o sistema faz e para quem.}}

## Motivação da migração

{{resposta do usuário ou "a confirmar"}}

## Prazo

{{resposta do usuário ou "a confirmar"}}

## Situação atual

- Plataforma: {{linguagem e runtime atuais}} — ver `.planning/migracao/STACK.md`
- Arquitetura: {{resumo em 1 linha}} — ver `.planning/migracao/ARQUITETURA.md`
- Complexidade da migração: {{nível}} — ver `.planning/migracao/ANALISE-CONSOLIDADA.md`

## Alvo

- Plataforma: {{linguagem e runtime alvo, conforme o CLAUDE.md e a `base-backend/`, ou "a confirmar"}}
- Arquitetura: Clean Architecture com DDD, na forma da referência do projeto (`base-backend/`)
- Camadas: {{as da referência do projeto, ou "a confirmar"}}
- Back-end de referência: `base-backend/` — {{o que ela define, com `caminho:linha`}}
- Front-end de referência: {{`base-frontend/` — o que ela define, com `caminho:linha`; ou "o sistema não tem front-end"}}
- Legado mapeado: `base-legado/` (somente leitura)

## Escopo

- Primeiro contexto a migrar: {{contexto}}
- Fora do escopo: {{lista ou "a confirmar"}}

## Restrições (o que não pode mudar)

- {{contratos de API, nomes de tabelas, integrações}}

## Integrações externas

| Integração | Decisão | Fonte |
|---|---|---|
| {{integração}} | {{mantém a interface / pode mudar / a confirmar}} | {{resposta do usuário}} |

## Regras implícitas confirmadas

| Regra | Ainda vale? | Fonte |
|---|---|---|
| {{I1}} | {{sim / não / a confirmar}} | DOMINIO.md |

## Roadmap

<!-- Uma linha por plano de migração. Comece pelo primeiro contexto escolhido na Fase 4; os demais seguem a "Ordem sugerida de migração" de ANALISE-CONSOLIDADA.md. -->

| # | Plano | O que é | Situação |
|---|---|---|---|
| 01 | {{contexto}} | {{uma frase}} | ⬜ a planejar |

## Principais riscos

- {{risco}} — ver ANALISE-CONSOLIDADA.md

## Documentos do mapeamento

| Arquivo | Conteúdo |
|---|---|
| `.planning/migracao/INVENTARIO.md` | Projetos, tamanho do código, primeiro commit |
| `.planning/migracao/STACK.md` | Tecnologias e versões |
| `.planning/migracao/ARQUITETURA.md` | Estrutura atual e violações |
| `.planning/migracao/DOMINIO.md` | Entidades, regras, contextos e aggregates |
| `.planning/migracao/QUALIDADE.md` | Testes, smells e débitos |
| `.planning/migracao/ANALISE-CONSOLIDADA.md` | Complexidade, riscos e decisões |
