<!--
Guia de preenchimento (apague este comentário no arquivo final):
- Use só os quatro documentos da Fase 2 e o INVENTARIO.md. Não releia o código.
- Cada afirmação cita o documento e a seção de origem, ou o caminho:linha que o documento traz.
- Classifique cada dimensão com os critérios abaixo. A complexidade geral é a PIOR dimensão.
- A seção "Decisões confirmadas" fica vazia até a Fase 4.

Critérios de complexidade por dimensão:

| Dimensão | Baixa | Média | Alta | Muito alta |
|---|---|---|---|---|
| Stack | Runtime e frameworks em versão com suporte, dependências mantidas | Versão anterior ainda suportada, ou poucas dependências sem suporte | Runtime ou framework fora de suporte, ou muitas dependências sem suporte | Plataforma descontinuada e acoplada ao código (framework de UI ou de servidor sem sucessor direto) |
| Arquitetura | Camadas separadas, sem violações | Violações pontuais | Regra de negócio em controllers e acesso a dados direto | Sem separação: tudo em um projeto, ciclos |
| Domínio | Contextos claros, regras no código | Contextos sobrepostos, poucas regras implícitas | Muitas regras implícitas ou espalhadas | Regras críticas no banco (SPs, triggers) ou desconhecidas |
| Qualidade | Testes no domínio, sem débitos críticos | Poucos testes, débitos altos pontuais | Sem testes, God Classes centrais | Sem testes e débitos críticos de segurança ou dados |
-->

# Análise consolidada

Unidade mapeada: `{{caminho}}` · Gerado em {{data}}

## Números

| Item | Valor | Fonte |
|---|---|---|
| Projetos | {{n}} | INVENTARIO.md |
| Linhas de código (sem gerados) | {{n}} | INVENTARIO.md |
| Primeiro commit | {{data}} | INVENTARIO.md |
| Plataforma | {{linguagem e runtime}} | STACK.md |
| Entidades / tabelas | {{n}} / {{n}} | DOMINIO.md |
| Testes | {{n}} | QUALIDADE.md |
| Débitos críticos / altos | {{n}} / {{n}} | QUALIDADE.md |

## Complexidade da migração: {{Baixa / Média / Alta / Muito alta}}

| Dimensão | Classificação | Justificativa |
|---|---|---|
| Stack | {{nível}} | {{achado}} — STACK.md |
| Arquitetura | {{nível}} | {{achado}} — ARQUITETURA.md |
| Domínio | {{nível}} | {{achado}} — DOMINIO.md |
| Qualidade | {{nível}} | {{achado}} — QUALIDADE.md |

Dimensão determinante: {{dimensão}}, porque {{motivo}}.

## Bounded contexts e aggregates propostos

| Contexto | Aggregates | Esforço | Dependências |
|---|---|---|---|
| {{Contexto}} | {{Aggregate (chave tipo)}} | {{P/M/G}} | {{contextos}} |

## Débitos técnicos críticos

1. {{débito}} — {{impacto na migração}} — QUALIDADE.md {{D#}}
2. {{débito}}
3. {{débito}}

## Riscos da migração

| Risco | Probabilidade | Impacto | Mitigação | Origem |
|---|---|---|---|---|
| {{risco}} | {{baixa/média/alta}} | {{baixo/médio/alto}} | {{mitigação}} | {{documento}} |

## Ordem sugerida de migração

1. {{Contexto}} — {{motivo: poucas dependências, valor, risco baixo}} — esforço {{P/M/G}}

## Lacunas do mapeamento

- {{o que ficou "não verificado" ou "não medido" e como resolver}}

## Perguntas em aberto

- {{perguntas reunidas das quatro análises}}

## Decisões confirmadas

<!-- Preenchido na Fase 4 com as respostas do usuário. -->

| Pergunta | Resposta | Data |
|---|---|---|
| {{pergunta}} | {{resposta}} | {{data}} |
