# Padrões de tela

Detalhe dos padrões de front-end do Senior Manager 2.0. As regras gerais (Vuexy, nenhuma biblioteca nova) estão no `CLAUDE.md`, P2. Leia este arquivo antes de criar ou mudar uma listagem, um formulário ou uma tela de hóspede.

## Telas-modelo da demo Vuexy

Demo: https://demos.pixinvent.com/vuexy-html-admin-template/html/vertical-menu-template/

| Caso | Páginas da demo |
|---|---|
| Cadastros e listas de pessoas | `app-user-list.html`, `app-user-view-*.html` |
| Documentos com itens, impressão | `app-invoice-list.html`, `app-invoice-edit.html`, `app-invoice-preview.html`, `app-invoice-print.html` |
| Perfil e configurações | `pages-profile-*.html`, `pages-account-settings-*.html` |
| Cursos, cards de conteúdo | `app-academy-*.html` |
| Painéis e indicadores | `cards-*.html` |
| Assistentes e modais | `modal-examples.html`, `form-wizard-*.html` |

A demo é referência de visual e estrutura. Ela não libera recursos fora de `src/SeniorManager.WebMvc/package.json` e `src/SeniorManager.WebMvc/src/libs/`.

## Grid padrão de listagem (listas pequenas)

Referência: `Areas/Administracao/Views/Unidades/Index.cshtml` e `Quartos.cshtml`. Para listas pequenas e limitadas, como unidades e quartos.

- `card` com `card-header` contendo o título (`h5.card-title`) e, à direita, os botões da tela (ex.: "Novo …" em `btn btn-primary` com ícone `tabler-plus`; em listas filhas, Voltar em `btn btn-label-secondary`).
- `div.card-datatable.table-responsive` com `table.table.table-striped.table-hover[data-datatable]`: DataTables em português, ordenado pela 1ª coluna, 25 por página.
- Zebrado e hover em tons do verde "success" do template, definidos em `wwwroot/css/site.css` (inclui a correção do conflito de zebrado e hover entre Vuexy e DataTables). Não crie outras cores para tabelas.
- Última coluna "Ações" com `th.nao-ordenar.text-end`, botões `btn btn-sm btn-text-*` com ícone Tabler e texto; ações de escrita como `form` POST, com `data-confirmar` (SweetAlert2) quando forem destrutivas.
- Mensagens de sucesso e erro via `data-notyf-sucesso` / `data-notyf-erro` (Notyf), lidas do `TempData` com `TempData.Consumir(...)`.

## Grid paginado no servidor (listas que crescem sem limite)

Referência: `Areas/Administracao/Views/Usuarios/Index.cshtml`. Para usuários, hóspedes, registros clínicos, faturas e tudo que cresce sem limite.

- Card "Filtros" acima da lista: formulário GET, com os filtros na URL e os botões Filtrar e Limpar.
- A tabela tem as mesmas classes e cores do grid padrão, mas sem `data-datatable`.
- Rodapé com a partial `Views/Shared/Componentes/_Paginacao.cshtml` (`PaginacaoViewModel`), que preserva os filtros.
- Caso de uso com `BasePagedRequest`/`BasePagedResponse`; o repositório faz `Count` e `Skip/Take` no banco.
- Depois de salvar, ativar ou inativar, volta para a mesma URL (`returnUrl` local).

## Componentes e scripts

- Os componentes são inicializados por `wwwroot/js/componentes.js` (atributos `data-*`).
- Os CSS/JS do template entram pelas partials `Views/Shared/Componentes/_VendorStyles.cshtml` e `_VendorScripts.cshtml`, nas seções `VendorStyles`, `VendorScripts` e `PageScripts` da View.
- Componentes por tipo: tabelas, DataTables; formulários, FormValidation; selects, Select2; datas, Flatpickr; alertas, SweetAlert2 e Notyf; gráficos, ApexCharts.
- CSS e JS próprios só para ajustes de uma tela, nunca reimplementando o que o template oferece.

## Breadcrumb (toda tela tem)

A maioria das telas com o layout do sistema exibe o breadcrumb do Vuexy (`nav[aria-label=breadcrumb] > ol.breadcrumb`), logo abaixo da barra superior e acima do conteúdo (salvo as exceções listadas abaixo). Referência de markup: `Views/Shared/Components/Breadcrumb/Default.cshtml` (demo `front-pages/help-center-article.html`).

- Trilho: **Área › Tela**, e nas telas filhas **Área › Lista › Tela** (ex.: `Financeiro › Faturas › Fatura`). Nas telas da ficha do hóspede: **`Administração › Hóspedes › Aba`**; no prontuário: **`Atendimento › Prontuário › Aba`** — o nome do hóspede **não entra no trilho** (ele já aparece no cabeçalho da ficha e do prontuário, logo abaixo, D-199).
- O último item é a página atual (`active`, `aria-current="page"`, sem link). Os itens intermediários que têm página própria são links; a área, que não tem página, é só texto.
- O título do último item é fixo por action, sem dado dinâmico como número ou nome (D-203).
- **Ficam sem trilho** (D-200): página de login, troca de senha, acesso negado, erro, telas de impressão (`_BlankLayout`), a página **Início** de cada área, a **Central de Ajuda** (`Ajuda/Index`) e a **Landing page** (Plano 22, layout `_FrontLayout`). Actions que só redirecionam sem renderizar view também ficam sem trilho (marcadas `SemTrilho: true` no catálogo para o teste de cobertura passar).
- O breadcrumb é gerado automaticamente pelo `BreadcrumbViewComponent` a partir do `CatalogoBreadcrumb` (`Navegacao/CatalogoBreadcrumb.cs`). As views não declaram nada. Uma action GET de tela sem entrada no catálogo faz um teste falhar (D-202).
- Uma tela nova sem entrada no catálogo não passa na Validação Opus.

## Formulários

Botões juntos no rodapé do card: **Salvar** (`btn btn-primary`) e, ao lado, **Voltar** (`btn btn-label-secondary`). O Voltar não vai no cabeçalho.

## Telas do hóspede

O hóspede vai sempre na URL, nunca na Session (CLAUDE.md, Contexto do produto).

- **Ficha cadastral** (área Administração desde 2026-11-01): rota `/Administracao/Hospedes/{hospedeId}/<Aba>`, com o cabeçalho `Areas/Administracao/Views/Hospedes/_Cabecalho.cshtml` (avatar, nome, status, leito e as abas em `nav-pills`) no topo de toda tela. Uma aba nova entra no `_Cabecalho` e em `AbaFicha`.
- **Prontuário clínico** (a partir do Plano 04), separado da ficha: rota `/Atendimento/Prontuario/{hospedeId}/<Aba>`, aberto aos perfis clínicos. Sinais vitais, evoluções, avaliações etc. são abas do prontuário, não da ficha. Uma aba nova entra no `Areas/Atendimento/Views/Prontuario/_CabecalhoProntuario.cshtml` e em `AbaProntuario`; o cabeçalho vem de `ToCabecalhoProntuarioDto()`.
- **Painel de seleção** (`/Atendimento/Prontuario`): os hospedados da unidade em cards, com "Só os meus" ligado.
