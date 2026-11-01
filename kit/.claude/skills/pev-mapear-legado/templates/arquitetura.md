# Guia — Arquitetura

Objetivo: descrever como o código está organizado hoje e onde ele se afasta de uma Clean Architecture. Leia STACK.md antes. Não repita as versões de dependências: cite STACK.md.

Não assuma a stack: descubra no projeto onde cada item fica e que padrões de Grep servem a ela (cite o padrão). Os exemplos entre parênteses são ilustrações, não uma lista a seguir.

## O que ler

1. **Grafo de módulos.** Dependências entre os módulos do projeto (projetos, módulos, pacotes, bibliotecas internas), lendo onde o build ou o código declara essas dependências. Monte o grafo e desenhe em Mermaid (`graph LR`). Sem módulos formais (um pacote só), desenhe o grafo das pastas principais pelos `import`/`require`/`use`.
2. **Camadas.** Classifique cada módulo pela função real dele, não pelo nome: apresentação, aplicação, domínio, infraestrutura/dados, cross-cutting, testes. Um módulo chamado `Domain` que depende do ORM está na camada de infraestrutura na prática.
3. **Violações de dependência.** Procure:
   - Domínio que depende de ORM, framework web, objetos de requisição/sessão ou leitura de configuração.
   - Controller ou handler que acessa o banco direto (contexto do ORM, conexão, repositório), sem camada de aplicação.
   - Ciclos no grafo de módulos.
   - Entidades do ORM expostas na API ou na tela, sem DTO ou ViewModel.
4. **Pontos de entrada.** Rotas, controllers e handlers, `main`, comandos de console, consumidores de fila, jobs agendados, hubs/websockets, endpoints mínimos. Conte controllers e rotas com Grep e cite o padrão.
5. **Injeção de dependência.** Onde os serviços são registrados e qual container é usado. Procure instanciação manual de serviços dentro de controllers e service locator. Em stacks sem container (ex.: Go, Ruby, PHP puro, Python), registre como as dependências são ligadas: construtores, módulos globais, `import` direto.
6. **Padrões de acesso a dados.** Repositório genérico, Unit of Work, ORM direto, SQL manual, stored procedures. Um ou vários contextos/conexões. Onde fica o mapeamento (arquivos de configuração, anotações, convenção). Migrations.
7. **Cross-cutting.** Autenticação e autorização, logging, tratamento de exceções (middleware, filtros, handlers globais), validação, mapeamento entre objetos, cache, multi-tenant.
8. **Fluxo de uma requisição.** Escolha um caso de uso representativo e siga da entrada (rota, controller, handler) até o banco. Cite cada passo com `caminho:linha`.

## Modelo

Copie a partir daqui para `.planning/migracao/ARQUITETURA.md`.

---

# Arquitetura atual

Unidade mapeada: `{{caminho}}` · Tecnologias: {{linguagens e build}} · Gerado em {{data}}

## Resumo

{{2 a 4 frases: estilo atual (camadas, monolito MVC, N-tier, funções soltas...), o quanto se aproxima de Clean Architecture e o maior problema estrutural.}}

## Grafo de módulos

```mermaid
graph LR
  {{ModuloA}} --> {{ModuloB}}
```

| Módulo | Camada real | Depende de | Evidência |
|---|---|---|---|
| {{nome}} | {{camada}} | {{módulos}} | `{{caminho:linha}}` |

## Violações de dependência

| Violação | Onde | Evidência | Esforço |
|---|---|---|---|
| {{domínio depende do ORM}} | {{módulo/classe}} | `{{caminho:linha}}` | {{P/M/G}} |

## Pontos de entrada

| Tipo | Quantidade | Padrão de contagem | Exemplos |
|---|---|---|---|
| Controllers / rotas | {{n}} | `{{padrão Grep}}` | `{{caminho:linha}}` |
| Jobs / workers / consumidores | {{n}} | `{{padrão Grep}}` | `{{caminho:linha}}` |

## Injeção de dependência

- Registro: `{{caminho:linha}}`
- Container: {{nome do container, ou "sem container: dependências ligadas por ..."}}
- Instanciação manual de serviços: {{lista com caminho:linha ou "não encontrada"}}

## Acesso a dados

- Padrão: {{repositório genérico / ORM direto / SQL manual / SP}} — `{{caminho:linha}}`
- Contextos ou conexões: {{lista}} — `{{caminho:linha}}`
- Mapeamento: {{arquivos de configuração / anotações / convenção}} — `{{caminho:linha}}`
- Migrations: {{ferramenta, quantidade e pasta, ou "não usa"}}
- Unit of Work / transações: {{como é feito}} — `{{caminho:linha}}`

## Cross-cutting

| Aspecto | Solução atual | Evidência |
|---|---|---|
| Autenticação | {{biblioteca ou mecanismo: sessão, JWT, cookie...}} | `{{caminho:linha}}` |
| Autorização | {{roles / policies / manual / só na tela}} | `{{caminho:linha}}` |
| Logging | {{biblioteca}} | `{{caminho:linha}}` |
| Exceções | {{handler global / filtro / try-catch espalhado}} | `{{caminho:linha}}` |
| Validação | {{biblioteca / anotações / manual}} | `{{caminho:linha}}` |
| Mapeamento | {{biblioteca / manual}} | `{{caminho:linha}}` |
| Multi-tenant | {{como isola os dados, ou "não existe"}} | `{{caminho:linha}}` |

## Fluxo de uma requisição: {{caso de uso}}

1. {{passo}} — `{{caminho:linha}}`

## Distância para Clean Architecture

| Princípio | Situação atual | Evidência |
|---|---|---|
| Domínio sem dependência de infraestrutura | {{atende / parcial / não atende}} | `{{caminho:linha}}` |
| Casos de uso isolados na camada de aplicação | {{...}} | `{{caminho:linha}}` |
| Infraestrutura implementa interfaces do núcleo | {{...}} | `{{caminho:linha}}` |
| Apresentação sem regra de negócio | {{...}} | `{{caminho:linha}}` |

## Perguntas para o usuário

- {{pergunta que o código não responde}}
