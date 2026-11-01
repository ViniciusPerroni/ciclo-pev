---
name: pev-planejar
description: Use para criar o plano de uma funcionalidade, módulo novo ou evolução de um módulo existente (que não vem de um sistema legado) em .planning/plano/ antes de qualquer código — quando o usuário pedir "planejar", "criar o plano", "próximo plano", "vamos fazer o módulo X", "evoluir o módulo X", ou descrever uma funcionalidade grande (várias telas, regras ou entidades), mesmo sem dizer "plano". Aceita como entrada uma história de usuário (arquivo, texto colado ou modelo templates/historia.md): com história, usa-a e pergunta só o que estiver nebuloso; sem história, faz o questionário completo. Também quando o usuário chamar pev-planejar. Para migrar um módulo que já existe no legado, use pev-planejar-migracao. Não use para implementar uma etapa de plano já aprovado nem para ajustes pequenos.
---

# PEV — Planejar

Esta skill produz um plano em `.planning/plano/NN-<slug>.md`, pronto para ser executado etapa por etapa, uma sessão por etapa (postulado P5 do `CLAUDE.md`).

**Por que o plano importa:** a implementação roda numa sessão nova por etapa, sem o histórico desta conversa. Tudo o que ela precisa saber (regras, decisões, telas, critérios) tem de estar escrito no plano. O que ficar vago aqui vira palpite lá.

**O plano diz o quê, não escreve o código.** Nada do código do projeto muda durante esta skill.

## Modelo e modo

Esta é a fase do Opus. Com `opusplan`, rode no modo de planejamento: lá não se gravam arquivos, então o plano é mostrado para aprovação e só é gravado depois que o usuário aprova (passo 6).

## Entrada: com história ou sem

O planejamento aceita as duas entradas, e o fluxo se adapta:

| Entrada | Como a skill trabalha |
|---|---|
| **Com história** | O usuário passou um arquivo (ex.: `.planning/historias/suporte.md`), colou um texto com histórias e critérios, ou escreveu no modelo `templates/historia.md`. A skill **usa a história como base** e faz o questionário do passo 3 **só nos pontos nebulosos**. |
| **Sem história** | Só o pedido ("vamos planejar o módulo X") e a linha do roadmap. A skill faz o **questionário completo**. |

Como saber qual é:
- o pedido traz um caminho de arquivo, ou um texto colado com "Como … quero … para …", critérios de aceitação ou regras? É história;
- na dúvida, procure em `.planning/historias/` um arquivo do tema. Se achar, confirme uma vez: "Encontrei `.planning/historias/x.md`. Uso como base?";
- a linha do roadmap no `PROJETO.md` **não conta** como história: é o resumo do que foi pedido e serve de contexto nos dois casos, mas não dispensa o questionário.

Uma história de qualquer tamanho serve, do rascunho de dois parágrafos ao modelo completo. Quanto mais completa, menos perguntas.

A história também diz o **tipo** do trabalho. Em **módulo novo**, o plano cria o contexto do zero. Em **evolução de módulo existente** ou **acabamento**, o centro do plano é o que já existe: telas, regras, perfis, testes, documentação e dados já gravados que mudam. Sem história, pergunte qual dos tipos é no começo da conversa.

## Passo 0 — Situar

Leia antes de perguntar qualquer coisa:

0. **A história**, se houver, inteira, inclusive os comentários e as caixas "A decidir".
1. `CLAUDE.md`: os postulados, o contexto do produto e os comandos do projeto.
2. `.planning/PROJETO.md`: a linha do roadmap deste plano, o escopo e as regras já confirmadas.
3. O plano mais recente em `.planning/plano/`, como modelo vivo do formato e do nível de detalhe, e os planos citados pela funcionalidade (ex.: um plano novo que fatura consumo depende do plano do consumo).
4. `.planning/criterios-base.md`: a lista de critérios de aceite que todo plano carrega.
5. Os números seguintes:
   - plano: o maior `NN` em `.planning/plano/` + 1;
   - decisões: o maior número achado com `Grep` (padrão `D-[0-9]+`, `output_mode: content`, `-o`) em `.planning/plano/*.md` + 1. Os números `D-` são globais e nunca se repetem.

## Passo 1 — Ponto de partida

Os planos de agora são, em geral, **módulos novos ou evoluções**, e a fonte das regras é o usuário, apoiado no que o sistema já tem. Faça as três buscas abaixo, delegando as maiores a subagentes `Explore` em paralelo.

**Com história:** a busca é guiada por ela. A seção "Integração com o que já existe" e o "Antes e depois" dizem onde procurar, mas **confira tudo no código**: a história pode estar desatualizada ou errada sobre o que existe. O que a história afirma e o código desmente vira uma pergunta no passo 3, com a evidência. Em **evolução**, a busca (a) é a mais importante: liste cada tela, caso de uso, regra de acesso, teste, artigo de documentação e dado já gravado que o plano vai mudar.

**a) O sistema atual (a busca principal).** Um módulo novo quase sempre se apoia no que já existe. Procure no código e nos planos anteriores as entidades, os casos de uso, as regras de acesso, os perfis, os avisos e os relatórios que o módulo vai ler, mudar ou estender, com `caminho:linha`. Isso vira a tabela "O que o sistema já tem" e mostra:
- os dados que o módulo reaproveita, em vez de recriar;
- as regras já decididas que ele precisa respeitar (cite o `D-NNN`);
- o que muda em telas ou regras existentes, e que portanto também muda a documentação delas.

**b) O legado (confirmação curta, se o projeto tem legado).** Confira se o legado não tem algum resto do tema: um campo solto, um relatório, uma tela abandonada. Uma busca rápida basta. Registre nas duas formas possíveis:
- achou algo: entra na tabela com `caminho:linha` e o destino no sistema novo;
- não achou: "busca em `base-legado/`: nenhuma entidade, migration ou tela de X". As regras são **funcionalidade nova** e vêm do usuário.

**c) Referências externas (quando o tema tiver).** Normas do setor, legislação, regras fiscais ou contábeis, práticas comuns de mercado. Use-as para **fazer perguntas melhores e propor opções**, nunca como regra pronta: o que vier daqui entra no plano só depois que o usuário aprovar, com a fonte citada.

## Passo 2 — Referências de interface (quando o plano tem telas)

Para cada tela nova, escolha a tela-modelo mais próxima dos padrões do projeto (o `CLAUDE.md` e `docs/padroes/` dizem quais são) e anote os componentes aproveitados. Confira se cada componente já existe no projeto. Se não existir, isso é uma pergunta ao usuário, não uma decisão sua. Plano sem telas pula este passo.

## Passo 3 — Entrevista e decisões

**Com história,** comece por "Com história: o mapa e só o que falta". **Sem história,** vá direto a "O questionário completo".

### Com história: o mapa e só o que falta

1. **Monte o mapa da história.** Para cada tema do questionário abaixo (quem usa, fluxo do dia a dia, estados, correção de erros, o que já existe, regras com exemplos, telas, relatórios e avisos, dados, fora do escopo, documentação), marque como a história o cobre:
   - ✅ **coberto:** está escrito e claro;
   - 🟡 **parcial:** existe, mas falta uma parte (diga qual);
   - ❌ **ausente:** a história não fala;
   - ⚠️ **conflito:** contradiz o sistema, o legado ou ela mesma (ponha a evidência `caminho:linha`).
2. **Não repita o que a história já decidiu.** Tema ✅ e regra "Decidida" viram decisões `D-NNN` direto, com a fonte `História (<arquivo>)`.
3. **Pergunte só:**
   - os "Pontos a decidir" e as regras "A decidir" da história;
   - os temas 🟡 e ❌;
   - os ⚠️, mostrando os dois lados com a evidência;
   - os critérios de aceitação sem exemplo concreto (peça um número ou uma data).
4. **Confirme a sua leitura numa rodada,** antes de escrever o plano: liste em poucas linhas as decisões que você tirou da história ("Entendi assim: …") e peça correção. Interpretar uma história errado custa mais que uma pergunta.
5. **A história é do usuário.** Não a reescreva nem "melhore" uma regra dela sem perguntar; uma sugestão sua entra como proposta "(Recomendado)" numa pergunta.
6. Se a história cobre quase tudo, são uma ou duas rodadas curtas, ou nenhuma além da confirmação.

### O questionário completo

Vale para o módulo sem história e, com história, para os temas que o mapa marcou 🟡, ❌ ou ⚠️.

Esta é a parte mais importante do plano: o usuário conhece a operação, e cada regra que não for perguntada será adivinhada na implementação. Antes das perguntas de detalhe, entenda o fluxo com o usuário:
- **quem usa:** quais perfis, se nasce perfil novo e o que cada perfil pode e não pode ver (dado sensível só para quem precisa);
- **o fluxo do dia a dia:** do começo ao fim, com um caso real ("um cliente entra no meio do mês, o que acontece com a cobrança?");
- **os estados:** situações pelas quais um registro passa, quem muda cada uma e se dá para voltar;
- **os erros:** como um registro errado é corrigido (anulação com motivo, estorno, cancelamento, rascunho descartável);
- **o que já existe:** como o módulo conversa com o que a busca do passo 1a encontrou;
- **o que fica de fora** nesta versão.

Depois, as perguntas de detalhe. Regra sem fonte não se inventa. Junte as dúvidas e pergunte de uma vez com a ferramenta de perguntas (`AskUserQuestion`, até 4 por rodada, várias rodadas se preciso). Em cada pergunta:
- dê um exemplo concreto com números ou datas, como o usuário faria ("entrou em 10/10, dia de cobrança 05: a primeira fatura cobra o quê?");
- traga a sua proposta como primeira opção, marcada "(Recomendado)", com o porquê.

Toda decisão vira uma linha da tabela "Decisões", com o número `D-NNN` e a fonte:
- `História (<arquivo>), confirmada pelo usuário (AAAA-MM-DD)`, quando a regra vem da história;
- `Resposta do usuário (AAAA-MM-DD)`;
- `Proposta minha, aprovada pelo usuário (AAAA-MM-DD)`;
- `Plano NN (D-xx)`, quando já foi decidida antes;
- `Legado (caminho:linha)`, quando o comportamento é mantido;
- `<Norma ou referência>, aprovada pelo usuário (AAAA-MM-DD)`, quando veio de uma referência externa.

O que o usuário não respondeu fica em "Perguntas em aberto" como `Q1`, `Q2`… com a sua proposta. Um plano com perguntas em aberto não é aprovado até elas serem resolvidas.

Pergunte também o que **não** entra: o "Fora do escopo" evita que a implementação cresça sozinha.

## Passo 4 — Escrever o plano

Use o modelo em `templates/plano.md`, com todas as seções. Seções sem conteúdo ficam com "Nenhuma" e o motivo, para mostrar que foram pensadas.

**Com história: rastreabilidade.** Mantenha a numeração da história (H1, H2…, CA1.1…) e cite-a nas "Entregas" das etapas ("implementa H2, CA2.1"), nos critérios de aceite do plano e nos testes. Cada história e cada critério de aceitação aparece em pelo menos uma etapa e vira teste ou conferência no sistema rodando: é a prova de que nada do que foi pedido ficou de fora. O "Fora do escopo" da história entra no do plano.

**Em evolução ou acabamento,** preencha a seção "Dados existentes e compatibilidade" do modelo: o que acontece com os registros já gravados (valor padrão, recálculo, carga ou nada), a migration e o teste dela, e as telas, regras e perfis que mudam, com a documentação de cada um. Em módulo novo, essa seção diz "Não se aplica".

### Etapas

As etapas são o que a implementação vai executar, uma por sessão. Para cada uma:

- **Título com tamanho e status:** `### Etapa 1 — Domínio, persistência e casos de uso · G · ⬜ pendente`. Tamanho P, M ou G. Uma etapa G é o máximo que cabe numa sessão; se ficar maior, divida.
- **Entregas:** o que existe no fim, citando as decisões (`D-NNN`) que cada entrega implementa.
- **Testes:** os casos concretos, com os exemplos das decisões (datas-limite, outro cliente, perfil barrado).
- **Critérios de pronto:** o que a Validação vai conferir nesta etapa.
- **Validação** como último item: o comando de testes do projeto verde e revisão do diff pelo agente `pev-revisor` (Opus).

Ordem que costuma funcionar (adapte ao caso):
1. Perfis e permissões, quando houver perfil novo;
2. Domínio, persistência e casos de uso, sem telas;
3. Telas e a documentação dessas telas;
4. Relatórios e avisos;
5. Dados de teste (seed) e fechamento (roadmap, conferência no sistema rodando);
6. **Revisão da documentação** (fixa, penúltima): ver abaixo;
7. **Validação final** (fixa, a última, tamanho P): ver abaixo.

As etapas 6 e 7 são fixas: entram em todo plano, mesmo pequeno. A 5 pode se juntar à 6 num plano pequeno. Cada etapa deixa o sistema compilando e com os testes verdes. Nenhuma depende de algo que só uma etapa posterior entrega.

### Revisão da documentação

A documentação de uma tela entra na mesma etapa da tela. A etapa de revisão existe porque as etapas seguintes mudam o comportamento depois que o texto foi escrito: uma regra ajustada na Etapa 3, uma decisão provisória, um aviso novo. Ela confere a documentação contra o sistema **como ficou**, não como foi planejado:
- cada artigo da seção "Documentação" do plano, relido contra as telas e as mensagens finais;
- os artigos de telas **existentes** que o módulo mudou (menu, perfis, navegação, outros módulos afetados);
- as perguntas frequentes e os testes de cobertura da documentação, se o projeto os tem.

Liste na etapa os artigos que ela vai revisar. Se o projeto não tem documentação de usuário, a etapa confere o README e as decisões técnicas.

### Validação final do plano

A Validação de cada etapa olha só o diff daquela etapa. Problemas que atravessam etapas (uma regra implementada pela metade, a documentação que não acompanhou uma mudança, um critério de aceite que nenhuma etapa cobriu) só aparecem olhando o plano inteiro. Por isso todo plano termina com a etapa **Validação final**, numa sessão própria:
1. O comando de testes verde e as migrations aplicando num banco limpo.
2. Revisão do diff completo da branch pelo agente `pev-revisor` (Opus), contra o plano inteiro: cada decisão `D-NNN` implementada, nada fora do escopo, os postulados.
3. Cada critério de aceite marcado ✅ com a evidência (nome do teste ou o que foi conferido) ou ❌ com o que falta.
4. Conferência no sistema rodando com os dados de teste, nos perfis do plano, incluindo um perfil que deve ser barrado.
5. As "Decisões provisórias" listadas para o usuário revisar.
6. O cabeçalho do plano passa a `✅ Concluído em AAAA-MM-DD`, com a branch e o número de testes, e a linha do roadmap é atualizada.

Achado da validação final é corrigido nela mesma, se for pequeno, ou vira uma etapa nova no plano, antes da conclusão.

### Critérios de aceite

Os critérios de aceite têm duas partes:
- **Do plano:** o comportamento verificável do módulo, em linguagem de usuário, cada um com o teste ou a conferência que o prova. Use os exemplos das decisões.
- **Base:** a lista de `.planning/criterios-base.md`. Copie a lista inteira. Item que não se aplica fica com "não se aplica" e o motivo, em vez de ser apagado.

### Seções que os postulados exigem

Confira no `CLAUDE.md` quais seções os postulados pedem em todo plano (ex.: referências de interface para plano com telas, documentação, arquitetura) e inclua-as.

## Passo 5 — Revisar antes de mostrar

Releia o plano como se você fosse quem vai implementar, numa sessão nova, executando a Etapa 2 sem esta conversa. Confira:

- [ ] O "Ponto de partida" mostra o que o módulo reaproveita do sistema (com `caminho:linha` e `D-NNN`) e o resultado da busca no legado, se houver.
- [ ] Toda tela ou regra existente que o módulo muda está numa etapa, com a documentação dela.
- [ ] *(Com história)* Toda história H-n e todo critério de aceitação aparecem numa etapa e num teste; o mapa da história não tem ❌ nem ⚠️ sem decisão; o usuário confirmou a sua leitura.
- [ ] *(Evolução)* Os registros já gravados têm tratamento (migration, valor padrão ou carga) e teste; a seção "Dados existentes e compatibilidade" está preenchida.
- [ ] Toda regra de negócio está numa decisão `D-NNN` com fonte. Nenhuma regra nasceu só da sua cabeça sem aprovação.
- [ ] Os exemplos numéricos das decisões viraram testes nas etapas.
- [ ] Segurança e isolamento aparecem nos testes de cada caso de uso e relatório, como o `CLAUDE.md` exige.
- [ ] Toda tela nova tem tela-modelo e componentes que já existem no projeto.
- [ ] Cada etapa tem tamanho, status, testes, critérios de pronto e Validação.
- [ ] As duas últimas etapas são a Revisão da documentação (com os artigos listados) e a Validação final.
- [ ] Os critérios de aceite têm a parte do plano (com a prova de cada um) e a base completa.
- [ ] Todo critério de aceite é coberto por alguma etapa.
- [ ] "Fora do escopo" está preenchido.

## Passo 6 — Aprovação e gravação

Mostre o plano e espere a aprovação. Ajuste e mostre de novo quantas vezes o usuário pedir.

Depois da aprovação:
1. Grave o plano em `.planning/plano/NN-<slug>.md`, com o cabeçalho `🟡 Aguardando aprovação` trocado por `🔄 Em execução` e todas as etapas `⬜ pendente`.
2. Atualize a linha do plano no `.planning/PROJETO.md`.
3. Se houve história em arquivo, escreva no topo dela `**Status:** Planejada (Plano NN)` e cite o arquivo na linha "Entrada" do plano. Se a história veio colada no pedido, ofereça salvá-la em `.planning/historias/<slug>.md` para o plano manter a origem.
4. Se o usuário quiser, crie a branch `planoNN/<slug>` a partir da branch principal e faça o commit `docs(planoNN): plano do <tema>, aprovado pelo usuário`.

Pare aqui: não comece a Etapa 0. Termine dizendo ao usuário para abrir uma sessão nova (ou `/clear`) e pedir: "Implemente a Etapa 0 de `.planning/plano/NN-<slug>.md`".
