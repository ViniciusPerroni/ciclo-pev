---
name: pev-executar
description: Use para implementar uma etapa de um plano aprovado em .planning/plano/ — quando o usuário pedir "implemente a Etapa 2 do plano 19", "continue o plano 19", "próxima etapa", "execute a etapa", "faça a revisão da documentação" ou "faça a validação final" de um plano, ou abrir uma sessão nova para retomar um plano. Também quando o usuário chamar pev-executar. Não use para criar ou mudar um plano (isso é pev-planejar ou pev-planejar-migracao) nem para ajustes pequenos fora de um plano.
---

# PEV — Executar Etapa

Esta skill implementa **uma** etapa de um plano aprovado, do começo ao commit, numa sessão própria (postulado P5 do `CLAUDE.md`). Ao terminar, o plano diz onde parou e a próxima sessão retoma dali sem o histórico desta conversa.

**O plano é o contrato.** O usuário aprovou o plano, não a sua interpretação dele. O que o plano decide, você implementa como está escrito. O que o plano não decide, você trata como no passo 3.

## Passo 1 — Situar

1. Encontre o plano: o caminho dado pelo usuário, ou o plano `🔄 Em execução` mais recente em `.planning/plano/`.
2. Encontre a etapa: a pedida, ou a **primeira que não está ✅**. Confira que as anteriores estão ✅. Se não estiverem, pare e diga ao usuário qual falta.
3. Confira o git:
   - a branch é a do plano (`planoNN/<slug>`). Se não for, pergunte antes de seguir;
   - `git status`: se houver mudanças que não são deste plano, pergunte o que fazer com elas. Você só vai fazer commit dos arquivos desta etapa.
4. Leia o plano **inteiro**, não só a etapa: as decisões `D-NNN` citadas, o "Ponto de partida" (nos planos de migração, "O que o legado faz", a "Matriz de cobertura" e a "Carga de dados"), o "Fora do escopo", os critérios de aceite e a "Execução" das etapas anteriores (o que já foi feito e as decisões provisórias já tomadas).
5. Leia o `CLAUDE.md` (postulados e comandos do projeto) e os arquivos citados no plano que esta etapa vai usar. Siga o padrão do que já existe no projeto (o plano anterior do mesmo tipo é a melhor referência de código).

Se for a primeira etapa do plano, troque o cabeçalho para `🔄 Em execução (branch \`planoNN/<slug>\`)`. Marque a etapa como `🔄 em andamento` no título.

Etapas especiais têm roteiro próprio: veja "Revisão da documentação" e "Validação final" no fim.

## Passo 2 — Implementar

Implemente as entregas da etapa seguindo os postulados do `CLAUDE.md`. O que mais escapa:
- **Interface:** antes de cada tela, abra a tela-modelo indicada no plano e use os componentes dela; nada fora do que o projeto já usa. Siga `docs/padroes/` quando o `CLAUDE.md` mandar.
- **Documentação:** a tela ou regra nova ou alterada ganha o texto de documentação **nesta mesma etapa**, não depois, se o projeto tem documentação de usuário.
- **Segurança e isolamento:** toda consulta nova respeita a regra de isolamento do projeto, e todo caso de uso novo tem o teste de "outro cliente / outra unidade / perfil sem permissão".
- **Migrations de banco:** pelo comando que o `CLAUDE.md` indica.

Escreva os testes que a etapa lista, com os exemplos das decisões, junto com o código e não no fim.

## Passo 3 — Quando o plano não responde

Vai acontecer. Separe em dois casos:

- **Detalhe de implementação, reversível e sem efeito na regra de negócio** (nome de um campo, onde fica um card, a ordem de uma lista, uma mensagem de erro no tom das outras): decida, siga em frente e registre nas "Decisões provisórias" do plano, com o porquê.
- **Regra de negócio, permissão, dado que o usuário vê ou algo que contradiz o plano**: pare e pergunte, trazendo a sua proposta como "(Recomendado)" e um exemplo concreto. Regra inventada é o erro que o fluxo mais quer evitar.

Na dúvida entre os dois, pergunte.

Se descobrir que o plano está errado (uma decisão impossível, uma dependência que não existe), não contorne em silêncio: explique ao usuário e proponha a correção do plano.

## Passo 4 — Testar

Rode o comando de testes do projeto (`CLAUDE.md`, seção Ambiente). Tudo verde antes da validação. Teste que já existia e quebrou é sinal de regra alterada: entenda por quê antes de mudar o teste.

Quando a etapa pedir conferência no sistema rodando, suba-o com os dados de teste e confira o fluxo nos perfis do plano, inclusive um que deve ser barrado. Olhe o log em busca de erros.

## Passo 5 — Validação

Chame o agente `pev-revisor` (ele roda no Opus) com:
- o caminho do plano e o número da etapa;
- o escopo: `git diff HEAD` mais os arquivos novos (`git status --porcelain`);
- um resumo do que você implementou e das decisões provisórias que tomou.

Para cada achado:
- **BLOQUEIA** e **CORRIGIR**: corrija. Se discordar de um achado, não o ignore: explique no relatório final por que não corrigiu.
- **SUGESTÃO**: aplique se for simples e estiver no escopo; senão, registre na Execução.

Depois de corrigir, rode os testes de novo. Se houve achado BLOQUEIA, chame o revisor mais uma vez sobre as correções. Se depois da segunda rodada ainda houver BLOQUEIA, pare e leve ao usuário.

## Passo 6 — Atualizar o plano

1. Título da etapa: `· ✅ concluída em AAAA-MM-DD`.
2. Seção **Execução**, uma entrada da etapa, no formato das anteriores: o que foi feito, os arquivos principais, as mudanças fora do previsto, os testes ("N novos, M no total, todos verdes"), o que a Validação encontrou e corrigiu, e a conferência no sistema, se houve.
3. **Decisões provisórias:** as desta etapa, continuando a numeração e agrupadas sob o título da etapa.
4. Se a etapa mudou algo que outra etapa ainda vai usar de outro jeito, ajuste aquela etapa no plano e diga isso no relatório.

## Passo 7 — Commit

Faça o commit da etapa: implementar a etapa inclui fechá-la. Adicione **só os arquivos desta etapa** com `git add <caminhos>` (nunca `git add -A` ou `git add .`, para não levar mudanças alheias). Mensagem no padrão do histórico do projeto:

```
feat(planoNN): <resumo da etapa> (Etapa N)
```

Não faça push nem merge.

## Passo 8 — Relatório

Termine com um relatório curto ao usuário:
- o que a etapa entregou, em linguagem simples;
- os testes (novos e total);
- o que a Validação encontrou e o que foi corrigido;
- as decisões provisórias tomadas, para ele revisar;
- o que ficou pendente ou mudou no plano;
- a próxima etapa, com a frase para a sessão nova: "`/clear` e depois: Implemente a próxima etapa de `.planning/plano/NN-<slug>.md`".

## Etapa especial — Revisão da documentação

Não há funcionalidade nova: a etapa confere a documentação contra o sistema como ficou.
1. Liste os artigos da etapa (novos e existentes afetados).
2. Para cada um, abra as telas e os casos de uso finais e confira: o passo a passo bate com a tela; as regras citam as mensagens que o usuário vê; o "quem pode" bate com as permissões; as decisões provisórias de todas as etapas estão refletidas.
3. Confira as perguntas frequentes e os testes de cobertura da documentação, se o projeto os tem.
4. Rode os testes, depois os passos 5 a 8.

## Etapa especial — Validação final

1. O comando de testes verde e as migrations aplicando num banco limpo.
2. Chame o `pev-revisor` em modo **Validação final**, com o escopo `git diff <branch-principal>...HEAD` e o plano inteiro.
3. Corrija os achados. Achado grande demais vira uma etapa nova no plano, antes desta, e a validação espera.
4. Suba o sistema com os dados de teste e confira os fluxos principais nos perfis do plano e num perfil que deve ser barrado.
5. Marque cada critério de aceite (do plano e da base) com ✅ e a evidência ou ❌ com o que falta. Plano com ❌ não é concluído.
6. Cabeçalho do plano: `✅ Concluído em AAAA-MM-DD (branch \`planoNN/<slug>\`; N testes)`. Atualize a linha do plano no `.planning/PROJETO.md`.
7. Commit `docs(planoNN): validação final` e o relatório, destacando as **decisões provisórias de todo o plano** para o usuário revisar e confirmar. O merge na branch principal é do usuário.
