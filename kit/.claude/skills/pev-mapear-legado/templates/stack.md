# Guia — Stack

Objetivo: registrar a tecnologia que o projeto usa hoje, com versões e evidência. Este documento responde "o que existe", não "o que está errado". Problemas vão para QUALIDADE.md.

Não assuma a stack: descubra no projeto onde cada item fica e que padrões de Grep servem a ela (cite o padrão). Os exemplos entre parênteses são ilustrações, não uma lista a seguir.

## O que ler

1. **Linguagem, runtime e build.**
   - Versão da linguagem/runtime e da ferramenta de build: o arquivo de versão da raiz (ex.: `global.json`, `.nvmrc`, `.python-version`, `go.mod`, `.ruby-version`), as propriedades herdadas por todos os módulos e o manifesto de cada módulo.
   - Tipo de cada módulo: aplicação web, API, worker, console, biblioteca.
   - Sinais de plataforma antiga ou sem suporte, com a evidência.
2. **Dependências.**
   - Manifesto de cada módulo, e o lockfile ou arquivo de gestão central quando for ele quem fixa a versão.
   - Agrupe por categoria: acesso a dados, web/API, autenticação, logging, mapeamento, validação, mensageria, testes, outros.
   - Registre dependências obsoletas ou sem suporte só se a evidência estiver no projeto; a avaliação fica em QUALIDADE.md.
3. **Banco de dados.** SGBD, driver, ORM ou SQL direto, stored procedures, ferramenta de migrations. Busque com Grep os padrões de acesso a dados que o projeto usa.
4. **Configuração.** Arquivos de configuração, arquivos de ambiente e variáveis lidas no código. Registre só os **nomes** das chaves e das seções. Nunca os valores.
5. **Front-end.** Templates do servidor, framework de SPA, `package.json`, bundler, bibliotecas de UI. Registre frameworks e versões.
6. **Infra e entrega.** `Dockerfile`, `docker-compose*.yml`, pasta `infra/`, IaC (Terraform, Helm), pipelines (`.github/workflows/*`, `azure-pipelines.yml`, `.gitlab-ci.yml`, `Jenkinsfile`), scripts de deploy.
7. **Integrações externas.** Clientes HTTP, SDKs de terceiros (e-mail, pagamento, storage, filas), clientes SOAP/WSDL, gRPC, brokers.

## Modelo

Copie a partir daqui para `.planning/migracao/STACK.md`.

---

# Stack atual

Unidade mapeada: `{{caminho}}` · Tecnologias: {{linguagens e build}} · Gerado em {{data}}

## Resumo

- Linguagem e runtime: {{linguagem e versão}} — evidência: `{{caminho:linha}}`
- Versão fixada na raiz: {{versão ou "não fixada"}} — `{{arquivo:linha}}`
- Ferramenta de build: {{nome e versão}}
- Tipo de aplicação: {{web MVC / API / worker / console / SPA / biblioteca}}
- Banco de dados: {{SGBD}} via {{ORM / driver / SQL direto}}

## Módulos

| Módulo | Tipo | Linguagem e versão | Evidência |
|---|---|---|---|
| {{nome}} | {{tipo}} | {{versão}} | `{{caminho:linha}}` |

Configuração herdada por todos os módulos: {{propriedades globais, ex.: modo estrito, versão de linguagem; ou "não existe"}}

## Dependências

Versão fixada em: {{manifesto de cada módulo / arquivo central / lockfile}}

| Categoria | Dependência | Versão | Módulos | Evidência |
|---|---|---|---|---|
| {{categoria}} | {{nome}} | {{versão}} | {{módulos}} | `{{caminho:linha}}` |

Dependências com versões diferentes entre módulos: {{lista ou "nenhuma"}}

## Acesso a dados

| Tecnologia | Onde | Evidência |
|---|---|---|
| {{ORM / driver / SQL direto / SP}} | {{módulo ou classe}} | `{{caminho:linha}}` |

Migrations: {{ferramenta e pasta, ou "não usa"}}

## Configuração

| Arquivo | Seções e chaves (sem valores) |
|---|---|
| `{{arquivo de configuração}}` | {{ConnectionStrings:Default, Jwt:Key...}} |

Segredos em arquivo versionado: {{arquivo e nome da chave, ou "nenhum encontrado"}}

## Front-end

| Tecnologia | Versão | Evidência |
|---|---|---|
| {{templates do servidor / React / Vue / Angular / jQuery...}} | {{versão}} | `{{caminho:linha}}` |

## Infra e entrega

| Item | Detalhe | Evidência |
|---|---|---|
| {{Dockerfile / pipeline / compose}} | {{imagem base, etapas}} | `{{caminho:linha}}` |

## Integrações externas

| Integração | Tipo | Onde é chamada | Evidência |
|---|---|---|---|
| {{serviço}} | {{HTTP / SOAP / gRPC / SDK / fila}} | {{classe ou módulo}} | `{{caminho:linha}}` |

## Perguntas para o usuário

- {{pergunta que o código não responde}}
