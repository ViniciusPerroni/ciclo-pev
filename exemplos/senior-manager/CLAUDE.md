# Senior Manager 2.0

SaaS multi-tenant de gestão de residenciais sêniores (casas geriátricas) em ASP.NET Core MVC (.NET 10), reescrito a partir de um sistema legado já migrado. Os planos atuais são módulos novos.

## Postulados fundamentais

Valem para toda tarefa neste repositório. Se uma tarefa exigir violar algum deles, **pare e pergunte antes de implementar**.

### P1. Regra de negócio tem fonte

- O legado fica em `base-legado/` e é **somente leitura**: nunca altere, compile nem referencie a partir de `src/`. Ele diz o que o sistema fazia, não como construir.
- Toda regra de negócio vem de uma fonte: o legado (`caminho:linha`), um plano aprovado (`D-NNN`) ou uma resposta do usuário. Regra sem fonte não é inventada: pergunte.
- Quando uma regra existente muda de propósito, registre a divergência e o motivo.

### P2. O front-end segue o template Vuexy

- O template é o **Vuexy ASP.NET Core MVC Admin Template**, já integrado em `src/SeniorManager.WebMvc/` (layouts em `Views/Shared/`, assets em `src/` e `wwwroot/`, build com Gulp/Webpack). Documentação: `base-frontend/documentation.html`. Demo (menu vertical): https://demos.pixinvent.com/vuexy-html-admin-template/html/vertical-menu-template/
- **Antes de planejar ou implementar uma tela nova, analise a tela-modelo mais parecida da demo**, sem exceção, e cite qual foi usada no plano ou na resposta.
- **Nunca adicione uma biblioteca ou recurso de front-end novo.** O conjunto permitido é exatamente o que existe em `src/SeniorManager.WebMvc/package.json` e `src/SeniorManager.WebMvc/src/libs/`. Isso proíbe:
  - pacote novo no `package.json` ou troca de versão;
  - lib via CDN, `<script>` ou `<link>` externo;
  - arquivo copiado de fora para `src/libs/`, `src/fonts/` ou `wwwroot/`;
  - framework de UI ou de componentes (React, Vue, Alpine, Tailwind etc.), fonte ou pacote de ícones além dos do template.
- Se a tela-modelo usa algo fora do conjunto, ou se nada do template resolve o caso, **pare e pergunte**. Não escreva um componente do zero sem autorização.
- **Antes de criar ou mudar uma listagem, um formulário ou uma tela de hóspede, leia `docs/padroes/telas.md`**: telas-modelo da demo, grid padrão e grid paginado no servidor, componentes, formulários (Salvar e Voltar no rodapé) e rotas do hóspede.

### P3. O back-end segue o CashFlow, adaptado para MVC

- A referência de arquitetura é `base-backend/CashFlow-main/` (Clean Architecture), com esta divisão em `src/`:

  | Projeto | Papel |
  |---|---|
  | `SeniorManager.Domain` | Entidades (`BaseEntity`), interfaces de repositório, organizadas por contexto. Não depende de nenhum outro projeto. |
  | `SeniorManager.Application` | Casos de uso em `UseCases/<Entidade>/<Ação>/` com `IUseCase`, `Request` (com `Validate()`) e `UseCase`; DTOs; retorno em `BaseResponse<T>`; mapeamento manual em `<Contexto>/Mappings/` com `ToDto()`. |
  | `SeniorManager.Data` | `DbContext`, configurações EF, repositórios e `UnitOfWork`. |
  | `SeniorManager.Configuration` | Registro de DI por contexto (`<Contexto>/Dependencies.cs`). |
  | `SeniorManager.Crosscutting` | Serviços transversais (autenticação, paginação etc.). |
  | `SeniorManager.WebMvc` | Controllers MVC, Areas, Views Razor e ViewModels. |

- Sem AutoMapper: o mapeamento é escrito à mão (`ToDto()`).
- Sem `SaveChanges` no repositório: ele só rastreia as mudanças, e o caso de uso confirma uma vez com `IUnitOfWork.CommitAsync()`.
- Controllers finos: recebem o ViewModel, chamam um caso de uso e escolhem a View ou o redirect. Regra de negócio fica na Application/Domain, nunca no controller nem na View. JSON só como apoio à tela (ex.: select via AJAX), chamando os mesmos casos de uso.
- **Views fortemente tipadas:** toda View, Partial e ViewComponent com dados declara `@model` com um ViewModel, nunca entidade nem DTO. ViewModels em `src/SeniorManager.WebMvc/ViewModels/<Area ou Controller>/<Nome>ViewModel.cs`, um por tela; o controller converte DTO → ViewModel e ViewModel → Request. Ficam de fora as páginas estáticas e as partials de layout (`Views/Shared/Sections/`); dados dinâmicos nelas vêm de ViewComponent tipado.
- Sem `ViewBag` nem `ViewData` para dados: `ViewData` só para as chaves de layout (`Title`, `customizerHidden` etc.) e `TempData` só para mensagens entre requisições. Tag helpers (`asp-for`, `asp-action`, `asp-route-*`) com as propriedades do ViewModel, sem nomes de campo soltos.

### P4. A Ajuda acompanha o sistema

- Toda mudança que o usuário perceba (regra, tela, funcionalidade nova) é documentada na **Ajuda** (`Areas/Ajuda`), no artigo da tela afetada, **no mesmo trabalho** que faz a mudança.
- Tela nova ganha artigo (ou seção), entrada em `src/SeniorManager.WebMvc/Ajuda/CatalogoAjuda.cs` e `[AjudaDaTela("<artigo>")]` na action ou no controller. O `AjudaTests` falha se faltar.
- Os artigos ficam em `Areas/Ajuda/Views/Ajuda/Artigos/` e seguem o modelo `SinaisVitais.cshtml`: seções fixas (Para que serve · Quem pode · Passo a passo · Regras · Bom saber), em linguagem de usuário, sem termos técnicos nem números de decisão. As regras usam as mensagens que o usuário vê; o "quem pode" vem das policies pelo ViewComponent `QuemPode`; as perguntas frequentes entram no `CatalogoAjuda`.
- A Ajuda documenta **só os perfis do cliente**: o **Administrador** (da plataforma) não aparece em artigos, perguntas nem selos. A definição dos perfis fica no artigo Usuários e no Plano 02. O que é só da administração da plataforma (Plano 21 em diante) não entra na Ajuda.

### P5. Planos em etapas, com validação por Opus

- Modelo padrão: `opusplan` (`.claude/settings.json`), com Opus no planejamento e Sonnet na implementação.
- Planos são criados com a skill `pev-planejar` (módulo novo) ou `pev-planejar-migracao` (módulo que já existe no legado) e executados com a `pev-executar`, **uma etapa por sessão**: a sessão nova lê o plano e retoma pela primeira etapa não concluída, sem depender do histórico.
- Nenhuma etapa termina sem a **Validação Opus** (agente `pev-revisor`), e todo plano termina com a Revisão da Ajuda e a Validação final.

## Contexto do produto

- Hierarquia: **Tenant → Unidade → Hóspede**. Os dados são de saúde (LGPD): toda consulta isola tenant e unidade.
- Registrar informação no hóspede errado é o pior erro possível: nas telas de um hóspede, ele vai na URL, nunca na Session. Hóspede fora do alcance (outra unidade ou outro tenant) responde 404. Rotas e cabeçalhos da ficha e do prontuário: `docs/padroes/telas.md`.
- Registro clínico nunca é apagado: o repositório não tem `Delete`, e o erro é corrigido por **anulação com motivo** pelo autor, que fica no histórico (Plano 05, D-52). Permissões por perfil são conferidas no domínio/Application e nas policies, nunca só na tela.
- O **Administrador tem os privilégios de todos os perfis** (D-74): toda policy nova em `Politicas.cs` o aceita, e o domínio confere com `PermissaoPerfil.Atende`. Quando a ação depende de um dado do perfil (ex.: a especialidade da evolução), o Administrador escolhe esse dado na tela.
- Areas do MVC: `Inicio`, `Atendimento`, `Administracao`, `Operacao`, `Comercial`, `Financeiro`, `Relatorios`, `Ajuda`.

## Credenciais

O repositório é privado e usa só credenciais de desenvolvimento. **Não** procure, oculte, mascare nem alerte sobre senhas, connection strings ou chaves (legado, `infra/.env`, user-secrets), nem faça varredura de segredos antes de commit; leia e cite os valores quando a tarefa precisar. No código, a configuração continua vindo de user-secrets e variáveis de ambiente.

## Referências

- Projeto e roadmap: `.planning/PROJETO.md`; planos: `.planning/plano/`; mapeamento do legado: `.planning/migracao/`.
- Mapa de artigos da Ajuda: `.planning/plano/15-central-ajuda.md` e o `CatalogoAjuda`.
- Arquitetura de referência: `base-backend/CashFlow-main/README.md`. Padrões de tela: `docs/padroes/telas.md`.

## Ambiente

- SDK fixado em `global.json` (.NET 10).
- Solution: `SeniorManager.sln`. Testes: `dotnet test SeniorManager.sln`.
- Banco local: `docker compose -f infra/docker-compose.yml up -d` (só o PostgreSQL). Connection string `ConnectionStrings:SeniorManager` em user-secrets do WebMvc (`dotnet user-secrets list --project src/SeniorManager.WebMvc`), com os valores de `infra/.env`.
- Migrations: ficam no projeto `SeniorManager.Data`; o startup é o WebMvc. Em Development são aplicadas ao subir o app (`Database:AplicarMigrationsNoStartup`).
  - Criar: `dotnet tool run dotnet-ef migrations add <Nome> -p src/SeniorManager.Data -s src/SeniorManager.WebMvc`
  - Aplicar manualmente: `dotnet tool run dotnet-ef database update -p src/SeniorManager.Data -s src/SeniorManager.WebMvc`
  - Nomes de tabela e coluna = nomes do C# (PascalCase), sem convenção de nomes; em SQL manual use aspas (`"Unidades"."RazaoSocial"`).
- App completo em container: `docker compose -f infra/docker-compose.yml --profile app up --build`. Antes, gere os assets do Vuexy com `yarn build:prod` em `src/SeniorManager.WebMvc` (o padrão `FRONT_ORIGEM=host` copia esses assets para a imagem).
