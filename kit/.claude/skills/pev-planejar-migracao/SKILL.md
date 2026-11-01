---
name: pev-planejar-migracao
description: Use para criar o plano de MIGRAÇÃO de um módulo de um sistema legado para o sistema novo, em .planning/plano/ antes de qualquer código — quando o usuário pedir "planejar a migração de X", "migrar o módulo X do legado", "plano de migração", ou quando a funcionalidade já existe no legado e precisa ser reproduzida no sistema novo, com as regras, telas e dados do legado como fonte. Também quando o usuário chamar pev-planejar-migracao. Para um módulo que NÃO existia no legado (funcionalidade nova), use pev-planejar. Não use para implementar uma etapa de plano já aprovado.
---

# PEV — Planejar Migração

Esta skill produz um plano de migração em `.planning/plano/NN-<slug>.md`, pronto para ser executado etapa por etapa pela `pev-executar`, uma sessão por etapa (postulado P5 do `CLAUDE.md`).

**A diferença para a `pev-planejar`:** lá o módulo é novo e as regras vêm do usuário. Aqui o módulo **já existe no legado** e a fonte das regras é o código dele. O legado diz **o que** o sistema faz (regras, fluxos, campos, validações, dados), não **como** construir: a arquitetura dele não é copiada. O trabalho do plano é: levantar tudo o que o legado faz, decidir o que se mantém, o que muda e o que não migra, e planejar a carga dos dados.

**Princípio:** toda regra do plano cita `caminho:linha` do legado, ou uma decisão do usuário. Regra sem evidência no legado não é inventada: pergunte. O pior erro numa migração é perder uma regra em silêncio, então o plano tem uma **matriz de cobertura** que não deixa nada de fora sem uma decisão.

**Por que o plano importa:** a implementação roda numa sessão nova por etapa, sem o histórico desta conversa. Tudo o que ela precisa (regras do legado com a evidência, decisões, telas, critérios) tem de estar escrito no plano.

**O plano diz o quê, não escreve o código.** Nada do código novo muda durante esta skill, e a pasta do legado é somente leitura.

## Modelo e modo

Fase do Opus. Com `opusplan`, rode no modo de planejamento: o plano é mostrado para aprovação e só é gravado depois que o usuário aprova (passo 6).

## Passo 0 — Situar

Leia antes de perguntar qualquer coisa:

1. `CLAUDE.md` (postulados, onde fica o legado, comandos do projeto) e `.planning/PROJETO.md`: a linha do roadmap deste plano, o escopo, as restrições e as **regras implícitas já confirmadas**.
2. O mapeamento do legado em `.planning/migracao/` (gerado pela `pev-mapear-legado`):
   - `INVENTARIO.md`: quais entidades, controllers, telas e relatórios pertencem ao módulo;
   - `DOMINIO.md`: as entidades, as regras implícitas (`I-n`) e as pendências ligadas ao tema;
   - `QUALIDADE.md`: os débitos (`D-n`) que o módulo toca (autorização ausente, regra só na tela etc.);
   - `ARQUITETURA.md` e `ANALISE-CONSOLIDADA.md`: o que já foi decidido sobre a migração.
   Se `.planning/migracao/` não existir, pare e sugira rodar a `pev-mapear-legado` primeiro.
3. O plano mais recente em `.planning/plano/` (modelo vivo do formato) e os planos que o módulo usa.
4. `.planning/criterios-base.md`.
5. Os números seguintes:
   - plano: o maior `NN` em `.planning/plano/` + 1;
   - decisões: o maior número achado com `Grep` (padrão `D-[0-9]+`, `output_mode: content`, `-o`) em `.planning/plano/*.md` + 1. Os números `D-` são globais e nunca se repetem.

## Passo 1 — O que o legado faz

Esta é a parte mais pesada do plano. Delegue o levantamento a subagentes `Explore` ("very thorough"), **em paralelo, um por eixo**, cada um com a lista de arquivos do módulo tirada do `INVENTARIO.md`. Peça sempre `caminho:linha` e "não verificado" quando não achar.

| Eixo | O que levantar |
|---|---|
| **Dados** | Entidades e campos (tipo, obrigatório, tamanho, enums, relações), migrations e o schema real do banco. |
| **Regras** | Validações (no modelo, no controller, no serviço, só na tela), cálculos, estados e transições, regras em código comentado, permissões por perfil. |
| **Telas e fluxos** | Pontos de entrada (rotas, controllers, handlers, comandos), telas, campos exibidos, filtros, ações, o fluxo de cada tela. |
| **Relatórios, jobs e integrações** | Relatórios do módulo, tarefas agendadas, e-mails, integrações externas. |

Com o resultado, monte cinco coisas.

**a) A tabela "O que o legado faz"** (Comportamento · Evidência · No novo sistema). Uma linha por regra, campo relevante ou comportamento, cada uma com `caminho:linha`. A coluna "No novo sistema" começa em branco e é preenchida no passo 3.

**b) A matriz de cobertura.** A lista exaustiva de **tudo** o que o legado tem no módulo: cada ponto de entrada (rota, action, handler), tela, relatório, job e regra relevante. Cada item recebe um destino:
- **Migra igual**;
- **Migra com mudança** (cita a decisão `D-NNN`);
- **Não migra** (cita a decisão e o motivo);
- **Defeito corrigido** (cita a decisão).

Nenhum item fica sem destino: é isso que garante que a migração não perde nada sem alguém ter decidido.

**c) Regras implícitas e defeitos.** Separe o que o legado faz sem dizer (regras `I-n` do `DOMINIO.md`, comportamento que só existe porque o código se comporta assim) e os defeitos (campo que nunca é salvo, regra em código comentado, validação só no navegador, ausência de autorização no servidor). Para cada um você vai perguntar se ainda vale (passo 3).

**d) Dependências do sistema novo.** O que os planos anteriores já entregaram e este plano usa: entidades, regras de acesso, perfis, avisos (cite `caminho:linha` e o `D-NNN`). É o que evita recriar o que já existe.

**e) A carga de dados.** Os dados do legado são migrados por script. Levante, para as tabelas do módulo: o mapeamento coluna a coluna para o novo schema; a conversão dos tipos (ex.: ids numéricos que mudam de tamanho ou viram UUID, datas e valores monetários); a conversão dos enums; valores nulos, órfãos ou inconsistentes que o script precisa tratar; e o volume, se for conhecido. Dado sensível: a carga respeita o isolamento definido nos postulados.

## Passo 2 — Referências de interface (quando o plano tem telas)

As telas do legado são referência de **conteúdo** (campos, ações, fluxos), não de visual. Para cada tela que o módulo vai ter, escolha a tela-modelo mais próxima dos padrões do projeto (o `CLAUDE.md` e `docs/padroes/` dizem quais são) e anote os componentes aproveitados. Confira se cada componente já existe. Se não existir, isso é uma pergunta ao usuário, não uma decisão sua.

## Passo 3 — Confirmar e decidir

Aqui o fluxo já existe: o legado o define. A conversa com o usuário **confirma e decide o que muda**, em vez de descobrir o fluxo. Junte as dúvidas por tema e pergunte com a ferramenta de perguntas (`AskUserQuestion`, até 4 por rodada, várias rodadas se preciso).

Em cada pergunta:
- cite o comportamento do legado com a evidência e um exemplo concreto ("no legado, `base-legado/.../Quarto:69-77`, a regra de 1 a 4 leitos está comentada. Hoje um quarto pode ter 6 leitos. Mantém o limite?");
- traga a sua proposta como primeira opção, marcada "(Recomendado)", com o porquê;
- ofereça sempre as saídas: **manter como está**, **mudar para X**, **não migrar**.

O que perguntar:
1. **Regras implícitas e defeitos** do passo 1c: "ainda vale?" para cada uma.
2. **Permissões:** o legado costuma checar o perfil só na tela. Quem pode fazer cada ação no sistema novo? Proponha uma regra por perfil, respeitando o que o `CLAUDE.md` define sobre dados sensíveis.
3. **O que não migra:** telas, relatórios ou campos que ninguém usa mais.
4. **Os dados:** o que da carga é descartado, corrigido ou preenchido com valor padrão.
5. **O que fica de fora** desta versão (vira o "Fora do escopo").

**O isolamento de dados definido nos postulados não se pergunta:** o sistema novo sempre o aplica, mesmo onde o legado não aplicava. Registre isso como divergência.

Toda decisão vira uma linha da tabela "Decisões", com o número `D-NNN` e a fonte:
- `Legado (caminho:linha)`, quando o comportamento é mantido;
- `Resposta do usuário (AAAA-MM-DD)`;
- `Proposta minha, aprovada pelo usuário (AAAA-MM-DD)`;
- `Plano NN (D-xx)`, quando já foi decidida antes.

**Toda divergência do legado é uma decisão com o motivo.** "Diverge do legado, onde …" fica escrito na própria decisão.

O que o usuário não respondeu fica em "Perguntas em aberto" como `Q1`, `Q2`… com a sua proposta. Plano com pergunta em aberto não é aprovado.

Terminadas as respostas, **preencha a coluna "No novo sistema"** da tabela do passo 1a e o destino de cada item da matriz de cobertura.

## Passo 4 — Escrever o plano

Use o modelo em `templates/plano.md`, com todas as seções. Seções sem conteúdo ficam com "Nenhuma" e o motivo.

O conjunto de regras sobre **etapas** (tamanho, entregas, testes, critérios de pronto, Validação), a **Revisão da documentação**, a **Validação final** e os **critérios de aceite** é o mesmo da `pev-planejar`. Leia as seções "Passo 4 — Escrever o plano" (Etapas, Revisão da documentação, Validação final do plano, Critérios de aceite) em `.claude/skills/pev-planejar/SKILL.md` e aplique-as. Aqui valem estas diferenças:

**Ordem das etapas numa migração** (adapte ao caso):
1. Perfis e permissões, quando mudam;
2. Domínio, persistência e casos de uso, sem telas, com as regras do legado;
3. Telas, menu e a documentação dessas telas;
4. Relatórios, avisos e jobs do legado;
5. **Carga de dados do legado** (script e verificação) e dados de teste (seed);
6. **Revisão da documentação** (fixa);
7. **Validação final** (fixa, a última).

**Testes que provam a paridade.** O legado quase nunca tem testes, então o plano cria os testes que provam o comportamento migrado: para cada regra "migra igual", um teste com o caso do legado (o exemplo numérico, a validação, o estado); para cada divergência, um teste do comportamento novo. Cite a decisão no nome ou na descrição do teste.

**Critérios de aceite:** além dos da lista base, o modelo traz três critérios de migração (paridade com a matriz, divergências registradas e carga de dados). Copie todos.

**Validação final:** além do que a `pev-planejar` pede, o `pev-revisor` confere a matriz de cobertura inteira: todo item "migra" está implementado e testado, e todo item "não migra" não apareceu.

## Passo 5 — Revisar antes de mostrar

Releia o plano como se você fosse quem vai implementar, numa sessão nova, executando a Etapa 2 sem esta conversa. Confira:

- [ ] Toda linha de "O que o legado faz" tem `caminho:linha` e o destino preenchido.
- [ ] A matriz de cobertura lista **todos** os pontos de entrada, telas, relatórios, jobs e regras do módulo no `INVENTARIO.md`, cada uma com destino.
- [ ] Toda divergência do legado é uma decisão `D-NNN` com o motivo.
- [ ] Toda regra "migra igual" tem um teste que reproduz o caso do legado.
- [ ] As regras implícitas e os defeitos têm decisão (ainda vale, corrige ou descarta).
- [ ] A carga de dados tem mapeamento, conversão de tipos e tratamento de dados inconsistentes, e vira uma etapa.
- [ ] As dependências do sistema novo estão citadas com `caminho:linha` e `D-NNN`.
- [ ] Segurança e isolamento aparecem nos testes de cada caso de uso e relatório, como o `CLAUDE.md` exige.
- [ ] Toda tela tem tela-modelo, componentes que já existem e documentação.
- [ ] Cada etapa tem tamanho, status, testes, critérios de pronto e Validação.
- [ ] As duas últimas etapas são a Revisão da documentação e a Validação final.
- [ ] Os critérios de aceite têm a parte do plano (com a prova de cada um) e a base completa, incluindo os de migração.
- [ ] "Fora do escopo" está preenchido.

## Passo 6 — Aprovação e gravação

Mostre o plano e espere a aprovação. Ajuste e mostre de novo quantas vezes o usuário pedir.

Depois da aprovação:
1. Grave o plano em `.planning/plano/NN-<slug>.md`, com o cabeçalho `🟡 Aguardando aprovação` trocado por `🔄 Em execução` e todas as etapas `⬜ pendente`.
2. Atualize a linha do plano no `.planning/PROJETO.md` e, se o plano respondeu regras implícitas, a tabela "Regras implícitas confirmadas".
3. Se o usuário quiser, crie a branch `planoNN/<slug>` a partir da branch principal e faça o commit `docs(planoNN): plano de migração de <tema>, aprovado pelo usuário`.

Pare aqui: não comece a Etapa 0. Termine dizendo ao usuário para abrir uma sessão nova (ou `/clear`) e pedir: "Implemente a Etapa 0 de `.planning/plano/NN-<slug>.md`".
