---
name: pev-mapear-legado
description: Use quando pedirem para mapear, analisar ou diagnosticar um projeto existente (brownfield), em qualquer linguagem ou stack, antes de migrá-lo para Clean Architecture e DDD, ou quando o usuário chamar pev-mapear-legado. Para um projeto novo, que ainda não existe, use pev-mapear.
---

# PEV — Mapear Projeto Existente

Esta skill é a etapa zero do fluxo PEV (Planejar · Executar · Validar): produz um mapeamento somente-leitura de um projeto existente em `.planning/migracao/`. O mapeamento é a base do plano de migração.

**Princípio:** cada afirmação do mapeamento cita `caminho:linha`. Sem evidência no código, escreva "não verificado". Um mapeamento com lacunas declaradas vale mais que um mapeamento completo e inventado.

**Agnóstica de linguagem:** a skill não assume stack nenhuma. Ela **descobre** a tecnologia do legado lendo o próprio projeto (manifestos, extensões, pastas, convenções) e registra o que achou com `caminho:linha`. Onde um guia pede um padrão de busca, derive-o do que o legado usa de fato e cite o padrão. A verdade técnica do **sistema novo** (linguagem, arquitetura, padrões) vem do `CLAUDE.md` e das bases de referência do projeto (`base-backend/` e, quando há interface, `base-frontend/`), que são **obrigatórias** (Fase 0), nunca desta skill. O legado a mapear é a pasta `base-legado/`, também obrigatória. O mapeamento descreve o legado; não decide o alvo.

## Ambiente (Claude Code)

- A tarefa principal lê arquivos, escreve `.md` e roda `git`.
- As análises rodam em subagentes, com a ferramenta `Agent` e `subagent_type: general-purpose`. O subagente só lê código e só escreve o `.md` de saída dele.
- Rode os subagentes um por vez, com `run_in_background: false`. Cada análise lê as anteriores, então a ordem importa.
- Os guias de cada análise ficam em `templates/`, na mesma pasta deste SKILL.md: `.claude/skills/pev-mapear-legado/templates/`, a partir da raiz do repositório. Passe caminhos absolutos para os subagentes.
- Para achar arquivos e buscar texto, use `Glob`, `Grep` e `Read`. Eles funcionam igual no Windows, macOS e Linux.
- Para comandos, use a ferramenta `Bash` (sintaxe POSIX) ou `PowerShell`. O Windows PowerShell 5.1 não aceita `&&`: use `;` ou rode um comando por chamada.
- Para perguntar ao usuário, use `AskUserQuestion`.

## Regras

1. Não altere código, projetos nem configuração. Escreva só em `.planning/`. As pastas `base-legado/`, `base-backend/` e `base-frontend/` são somente leitura.
2. Não rode build, instalação de dependências, testes nem o sistema, seja qual for a ferramenta. Eles gravam em pastas de saída e podem baixar pacotes.
3. Nunca copie valores de segredos (connection strings, senhas, tokens, chaves de API) para os documentos. Cite o arquivo e o nome da chave. Os documentos vão para o git.
4. Métrica que você não mediu fica "não medido", com o comando ou ferramenta que mediria. Nunca estime cobertura, complexidade ciclomática nem acoplamento.
5. Esforço é tamanho relativo: P, M ou G. Nunca em horas ou dias.
6. Os modelos usam `{{...}}` para o que você preenche. Linha de modelo sem evidência no projeto: apague a linha.

## Fase 0 — Pré-requisitos obrigatórios

**Esta skill não começa sem as bases de referência.** Antes de qualquer outra coisa, confira na raiz do repositório com `Glob` (`base-legado/**`, `base-backend/**`, `base-frontend/**`). Uma pasta só conta se existir e tiver arquivos de verdade (um `.gitkeep` ou um `README` vazio não vale).

| Pasta | Obrigatória | Papel |
|---|---|---|
| `base-legado/` | **Sempre**, nesta skill | O sistema antigo, somente leitura. É o que será mapeado. |
| `base-backend/` | **Sempre** | Projeto(s) de referência do back-end do sistema novo: arquitetura e boas práticas. |
| `base-frontend/` | **Se o sistema envolve front-end** | Design System e/ou template de interface do sistema novo. |

1. **Pergunte sobre o front-end**, com `AskUserQuestion`: "O sistema envolve front-end (telas, interface web)?" Opções: "Sim", "Não, é só back-end (API, jobs, integrações)". Se o legado tiver telas (views, templates, páginas) e o usuário disse "Não", confirme de novo antes de aceitar.
2. **Se faltar qualquer pasta obrigatória, pare.** Não mapeie, não crie `.planning/migracao/`, não ofereça alternativa. Diga ao usuário, em poucas linhas:
   - quais pastas faltam e para que servem (tabela acima);
   - que elas ficam na raiz do repositório, com esses nomes;
   - que, ao colocá-las, ele pode chamar a skill de novo.

   O motivo: a verdade técnica do sistema novo vem dessas bases. Sem elas, o plano de migração teria de inventar arquitetura e interface.
3. **Com tudo no lugar,** leia o `README` e a estrutura de pastas de cada base de referência (`base-backend/`, e `base-frontend/` se houver) para registrar, no `INVENTARIO.md` e depois no `PROJETO.md`, o que elas definem (linguagem e runtime, camadas, padrões, componentes), com `caminho:linha`. Aponte o que **não encontrou**.

## Fase 1 — Verificação

1. **Mapeamento existente.** Liste `.planning/` com `Glob` (padrão `.planning/**`).
   - Se `.planning/PROJETO.md` ou `.planning/migracao/` existir, pare. Pergunte com `AskUserQuestion`. Opções: "Retomar: gerar só os arquivos que faltam", "Refazer do zero (sobrescreve)", "Cancelar". Espere a resposta.
2. **Raiz e tecnologias do legado.** O legado é a pasta `base-legado/`: mapeie só o que está nela e cite os caminhos a partir dela (`base-legado/...:linha`). Liste `base-legado/` com `Glob` (`base-legado/*`) e os arquivos de primeiro e segundo nível. Procure o que declara dependências, versões e build: arquivos de manifesto, de lock, de solução ou de workspace, `Makefile`, `Dockerfile`, pipelines. Leia-os para descobrir **a linguagem, o runtime e a ferramenta de build**; não parta de uma lista de stacks. Ignore pastas de dependências baixadas e de saída de build (as que o `.gitignore` e o próprio projeto indicam, como `node_modules/`, `vendor/`, `bin/`, `obj/`, `target/`, `build/`, `dist/`, `.venv/`).
   - Uma raiz de build: use essa. Manifestos em subpastas costumam ser módulos ou subconjuntos dela.
   - Mais de uma raiz dentro de `base-legado/`: pergunte qual mapear e ponha a mais externa como primeira opção.
   - Mais de uma linguagem ou stack (ex.: back-end e front-end): pergunte se mapeia tudo ou uma parte. Mapeando tudo, cada guia trata cada stack em uma subseção.
   - Nada reconhecível: pergunte qual é a tecnologia do legado. Sem resposta, pare.
3. **Inventário.** Crie `.planning/migracao/INVENTARIO.md` com estas seções:
   - **Tecnologias.** Linguagem(ns), versão de runtime e ferramenta de build, cada uma com a evidência (`caminho:linha`). Sem evidência: "não verificado".
   - **Módulos.** Liste as unidades de build ou de código que o projeto declara (projetos, módulos, pacotes, aplicações), com o caminho de cada uma. Liste também as que existem no disco e não estão declaradas no manifesto raiz.
   - **Tamanho do código.** Defina as extensões de código-fonte a partir do que você viu no projeto e exclua dependências, saída de build e código gerado (anote cada exclusão no inventário, com o motivo). Copie o total de linhas e os 20 maiores arquivos.
   - **Primeiro commit.** Rode `git log --max-parents=0 --format=%ad --date=short`. Sem repositório git, escreva "não verificado".
   - Se o total passar de 100.000 linhas, pergunte se mapeia tudo ou um subconjunto de módulos. Registre a resposta no inventário.

   Troque `<ext>` pelas extensões de código que você identificou e ajuste a lista de pastas excluídas ao projeto.

   Bash (dois comandos), exemplo com a extensão `<ext>`:

   ```bash
   find . -type f -name '*.<ext>' -not -path '*/.git/*' -not -path '*/<pasta-excluida>/*' -exec cat {} + | wc -l
   ```

   ```bash
   find . -type f -name '*.<ext>' -not -path '*/.git/*' -not -path '*/<pasta-excluida>/*' -exec wc -l {} + | grep -v ' total$' | sort -rn | head -20
   ```

   PowerShell (um comando), com `$ext` e `$excluir` ajustados ao projeto:

   ```powershell
   $ext = '*.<ext>'; $excluir = '[\\/](\.git|<pasta-excluida>)[\\/]'; $a = Get-ChildItem -Recurse -File -Filter $ext | Where-Object { $_.FullName -notmatch $excluir } | ForEach-Object { [pscustomobject]@{ Linhas = @(Get-Content -LiteralPath $_.FullName).Count; Arquivo = Resolve-Path -Relative $_.FullName } }; "Arquivos: $($a.Count)  Linhas: $(($a | Measure-Object -Property Linhas -Sum).Sum)"; $a | Sort-Object Linhas -Descending | Select-Object -First 20 | Format-Table -AutoSize
   ```

   Mais de uma extensão: repita o comando por extensão e some os totais. Repita `-not -path` (Bash) ou use `|` no regex (PowerShell) para cada pasta excluída. Para excluir arquivos gerados pelo nome, acrescente `-not -name '<padrão>'` (Bash) ou outra condição no `Where-Object` (PowerShell).

## Fase 2 — Quatro análises em subagentes

Rode as quatro análises nesta ordem. Cada uma é uma chamada `Agent` com `subagent_type: general-purpose` e `run_in_background: false`.

| Ordem | Guia | Saída |
|---|---|---|
| 1 | `templates/stack.md` | `.planning/migracao/STACK.md` |
| 2 | `templates/arquitetura.md` | `.planning/migracao/ARQUITETURA.md` |
| 3 | `templates/dominio.md` | `.planning/migracao/DOMINIO.md` |
| 4 | `templates/qualidade.md` | `.planning/migracao/QUALIDADE.md` |

Prompt de cada subagente. Troque os `<...>`; use caminhos absolutos:

```text
Você executa UMA análise somente-leitura de um projeto existente, como subagente da skill pev-mapear-legado.

Tema: <tema>. Unidade mapeada: <caminho da raiz ou da pasta>. Tecnologias: <as do INVENTARIO.md>.
Guia e modelo: leia <caminho absoluto da pasta da skill>/templates/<guia>.md e siga o guia.
Tecnologia: descubra no próprio projeto onde cada coisa fica e que padrões de Grep servem a esta stack. Teste o padrão numa amostra antes de citar a contagem.
Contexto já coletado: .planning/migracao/INVENTARIO.md e as análises anteriores em .planning/migracao/.
Saída: escreva só <caminho da saída>.

Regras:
- Use só Read, Glob, Grep e Write. Não rode comandos de shell. Não altere nenhum outro arquivo.
- Cada achado cita caminho:linha. Sem evidência, escreva "não verificado".
- Métrica que você não mediu fica "não medido". Nunca estime cobertura, complexidade nem acoplamento. Contagens feitas com Grep (output_mode count) valem como medidas: cite o padrão usado.
- Esforço é P, M ou G. Nunca horas ou dias.
- Nunca copie valores de segredos (connection strings, senhas, tokens, chaves). Cite o arquivo e o nome da chave.
- Apague as linhas do modelo que não têm evidência no projeto. Não deixe nenhum {{...}} no arquivo final.
- Se o projeto tiver mais de uma stack, trate cada uma em uma subseção.

Ao terminar, responda com no máximo 15 linhas: os números principais, os 3 achados mais importantes (com caminho:linha) e as perguntas que só o usuário responde.
```

Depois de cada subagente:

1. Confira se o arquivo de saída existe.
2. Busque `\{\{` no arquivo com `Grep`. Se achar, o subagente deixou modelo sem preencher: complete você ou apague a linha.
3. Guarde o resumo de 15 linhas para a Fase 3.

Se o subagente falhar ou o usuário recusar a chamada, faça a análise aqui mesmo com o mesmo guia. Depois siga para a próxima.

## Fase 3 — Consolidação

1. Leia os quatro arquivos da Fase 2. Não releia o código.
2. Preencha `templates/ANALISE-CONSOLIDADA.md` e grave em `.planning/migracao/ANALISE-CONSOLIDADA.md`.
3. Classifique a complexidade pela pior dimensão, com os critérios do modelo. Justifique com os achados.

## Fase 4 — Perguntas

Faça as perguntas com `AskUserQuestion`, uma pergunta por chamada. Tire as opções dos achados. O usuário sempre pode responder em texto livre.

1. Qual módulo ou bounded context migrar primeiro? (opções: os contextos de `DOMINIO.md`)
2. A migração mantém a linguagem e o runtime atuais (só reorganiza a arquitetura) ou muda de plataforma? Qual é a plataforma alvo?
3. O que não pode mudar? (contratos de API, nomes de tabelas, integrações)
4. As integrações externas mantêm a interface atual ou podem mudar?
5. Qual a motivação da migração e o prazo?
6. As regras de negócio implícitas (❓ em `DOMINIO.md`) ainda valem? Pergunte só se houver alguma.

Espere cada resposta. Registre as respostas na seção "Decisões confirmadas" de `ANALISE-CONSOLIDADA.md`.

## Fase 5 — PROJETO.md

Só depois das respostas da Fase 4. Preencha `templates/PROJETO.md` e grave em `.planning/PROJETO.md`. Campo sem resposta nem evidência fica "a confirmar".

## Fase 6 — Commit e entrega

Pule o commit em qualquer um destes casos:

- O projeto não é um repositório git.
- `.planning/config.json` tem `git.commit_docs: false`.
- `git check-ignore -v .planning/PROJETO.md` imprime uma regra. Isso quer dizer que `.planning/` está no `.gitignore`.

Senão, pergunte ao usuário se pode fazer o commit. Se ele aceitar, rode um comando por chamada:

```text
git add .planning/migracao .planning/PROJETO.md
```

```text
git commit -m "docs: análise do projeto para migração" -m "Arquivos: INVENTARIO, STACK, ARQUITETURA, DOMINIO, QUALIDADE, ANALISE-CONSOLIDADA e PROJETO." -- .planning/migracao .planning/PROJETO.md
```

Acrescente ao commit as linhas de atribuição que a sessão definir. O `--` com os caminhos limita o commit a esses arquivos. Mudanças que o usuário já tinha em stage ficam de fora.

Feche com uma mensagem final:

- Tabela dos arquivos criados e o propósito de cada um.
- Aggregates propostos (lista curta).
- Os 3 débitos técnicos críticos.
- Complexidade da migração.
- Próximo passo sugerido: montar o plano de migração do primeiro contexto escolhido na Fase 4.

## Erros comuns

| Erro | Correção |
|---|---|
| Assumir a stack pelo nome da pasta ou pelo README | Confirme nos arquivos de build e de dependências e cite `caminho:linha`. |
| Ler a versão do runtime só no manifesto do módulo | Um arquivo da raiz ou do módulo pai pode fixar a versão para o projeto todo. Procure nos dois níveis. |
| Ler a versão de uma dependência só no manifesto | Arquivos de lock e de gestão central podem guardar a versão real. Confira. |
| Ignorar sinais de plataforma antiga | Runtime ou framework fora de suporte aumenta a migração. Registre a evidência. |
| Contar código gerado como código | Migrations, código gerado por ferramenta e arquivos minificados ficam fora do inventário e das God Classes. |
| Contar dependências baixadas e saída de build | Exclua da contagem as pastas que o projeto gera ou baixa. |
| Copiar uma connection string ou valor de arquivo de ambiente para o documento | Cite o arquivo e a chave. O documento vai para o git. |
| Usar um padrão de Grep sem testá-lo | Teste o padrão numa amostra do projeto antes de citar a contagem. |
| Preencher cobertura ou complexidade "de olho" | Escreva "não medido" e o comando que mede. |
| Propor um tipo de chave novo para o aggregate | Use o tipo de chave atual da tabela. As tabelas não mudam. |
| Rodar as análises em paralelo | Cada análise lê as anteriores. Rode uma por vez. |
| Rodar as análises em paralelo | Cada análise lê as anteriores. Rode uma por vez. |
| `git commit` sem caminhos | O commit leva junto o que o usuário já tinha em stage. |
