# Critérios de aceite base

Esta lista vale para **todo plano** do projeto. A skill `pev-planejar` copia os itens para a seção "Critérios de aceite › Base" de cada plano, e a Validação final marca cada um com ✅ e a evidência, ou ❌ com o que falta. O que não se aplica a um plano fica com "não se aplica: <motivo>" (não apague o item: mostra que foi pensado).

**Como adaptar ao seu projeto:** os itens abaixo são genéricos. Troque os trechos entre `{{ }}` pelo que vale no seu projeto, apague o que não faz sentido e acrescente os itens do seu domínio. No fim há exemplos de itens de domínio. A versão do Senior Manager está em `exemplos/senior-manager/CLAUDE.md` e no plano de exemplo.

## Itens

- **Segurança e isolamento:** toda consulta, caso de uso e relatório novo filtra por {{cliente / empresa / unidade — o que isola dados no seu projeto}}; um registro de outro {{cliente}} responde "não encontrado" (teste com dois {{clientes}}, também em escrita).
- **Permissões:** cada regra de perfil é conferida no domínio ou na camada de negócio, não só na tela; os perfis que não podem usar são barrados (teste por perfil).
- **Arquitetura:** o código segue os postulados do `CLAUDE.md` (camadas, onde fica a regra de negócio, padrões de acesso a dados, injeção de dependências); nada de padrão fora do combinado sem decisão registrada.
- **Interface:** {{só componentes e bibliotecas do padrão do projeto; listas, formulários e mensagens no padrão de `docs/padroes/`}}. *(Remova se o projeto não tem interface.)*
- **Documentação do usuário:** {{cada tela ou regra nova ou alterada tem o artigo da ajuda/manual, escrito com as mensagens que o usuário vê}}. *(Remova se o projeto não tem.)*
- **Dados:** migration criada e aplicando num banco limpo; dados de teste ou seed cobrindo o módulo, repetíveis e testados.
- **Dados existentes (evolução):** os registros gravados antes da mudança continuam abrindo e somando certo (valor padrão, recálculo ou carga, com teste); em módulo novo, não se aplica.
- **História (se houve):** cada história H-n e cada critério de aceitação da história está coberto por uma etapa e por um teste ou conferência no sistema rodando.
- **Qualidade:** o comando de testes do projeto passa; o sistema foi conferido rodando, sem erro no log.
- **Documentação do plano:** status das etapas, seção Execução e Decisões provisórias preenchidos; o roadmap (`PROJETO.md`) atualizado.

## Exemplos de itens de domínio (para copiar e adaptar)

- **Registros que não se apagam:** registro {{clínico / financeiro / contábil}} emitido não tem exclusão; o erro se corrige por anulação, estorno ou cancelamento com motivo, que fica no histórico.
- **Valores monetários:** cálculos em tipo decimal, com a regra de arredondamento definida em decisão e coberta por teste; nunca ponto flutuante.
- **Fechamento de período:** lançamento em período fechado é recusado; a reabertura exige perfil e motivo, e fica registrada.
- **Trilha de auditoria:** quem fez, quando e o quê, em toda alteração de dado sensível.
- **Dados pessoais (LGPD):** dado sensível só aparece a quem precisa; nenhum dado real em log ou em dados de teste.
