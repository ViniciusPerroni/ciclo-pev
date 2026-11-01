<!--
Guia de preenchimento (apague este comentário no arquivo final):
- Preencha só depois da conferência da Fase 9.
- Fontes: respostas do usuário (com data), documentos de apoio (com o caminho) e bases de referência (com caminho:linha).
- Toda regra, decisão e restrição cita a fonte. Campo sem resposta nem evidência fica "a confirmar". Não invente prazo, motivação, regra nem tecnologia.
- Este é o documento de entrada de todo plano (pev-planejar). Seja curto: detalhe fica nos documentos de apoio e nos planos.
- Não copie segredos nem dados reais de documentos. Só a estrutura.
-->

# {{Nome do projeto}}

Atualizado em {{AAAA-MM-DD}}

## Visão

{{1 a 3 frases: o que o sistema faz e para quem.}}

## Problema e sucesso

- **Como é resolvido hoje:** {{resposta ou "a confirmar"}}
- **Motivação:** {{resposta ou "a confirmar"}}
- **Prazo:** {{data, marco, "sem prazo" ou "a confirmar"}}
- **Como saber que deu certo:** {{1 a 3 resultados observáveis, ou "a confirmar"}}

## Usuários e perfis

| Perfil | Quantos | O que faz | O que não pode ver ou fazer | Fonte |
|---|---|---|---|---|
| {{perfil}} | {{n}} | {{ações}} | {{restrições}} | {{resposta / documento}} |

- **Hierarquia e isolamento:** {{ex.: Empresa → Unidade → Usuário; como os dados se isolam, ou "organização única"}}
- **Autenticação:** {{como os usuários entram, ou "a confirmar"}}
- **Dados sensíveis:** {{o que é sensível e a regra (LGPD, sigilo, auditoria), ou "nenhum, confirmado pelo usuário em AAAA-MM-DD"}}
- **Regras invioláveis:** {{ex.: registro emitido nunca é apagado, só anulado com motivo; ou "nenhuma informada"}}

## Escopo da primeira versão

- {{funcionalidade ou módulo, em linguagem de usuário}}

**Fora do escopo:** {{lista ou "a confirmar"}}

## Entrada e saída de dados

### Como os dados entram

| Entrada | Quem fornece | Frequência | Volume | Formato | Dado inválido | Fonte |
|---|---|---|---|---|---|---|
| {{digitação em tela / importação de arquivo / API recebida / fila / dispositivo / carga inicial}} | {{quem}} | {{frequência}} | {{volume}} | {{formato}} | {{rejeita o lote / a linha / avisa}} | {{resposta / documento}} |

- **Carga inicial (dia zero):** {{de onde vêm os dados atuais, ou "não há"}}
- **Dados de teste:** {{amostras anonimizadas ou fictícios; cenários que precisam existir}}
- **Histórico e auditoria:** {{o que registra quem, quando e o quê, ou "a confirmar"}}

### O que o sistema gera

| Saída | Para quem | Formato ou canal | Fonte |
|---|---|---|---|
| {{relatório / exportação / impressão / e-mail / aviso / dado enviado a outro sistema}} | {{quem}} | {{formato}} | {{resposta / documento}} |

## Documentos de apoio

| Documento | Tipo | O que fornece | O que não cobre | Local |
|---|---|---|---|---|
| {{nome}} | {{briefing / requisitos / histórias / protótipo / modelo de dados / API / planilha / norma}} | {{o que foi aproveitado}} | {{lacunas}} | `{{caminho ou link}}` |

{{Ou: "Nenhum documento fornecido até {{AAAA-MM-DD}}. As respostas do usuário são a única fonte; vale trazer material antes do primeiro plano."}}

## Stack de referência

A tecnologia deste projeto vem das bases de referência. Divergência delas é decisão registrada, não improviso.

| Camada | Referência | Onde | O que ela define |
|---|---|---|---|
| Back-end | {{projeto de referência em `base-backend/`}} | `{{base-backend/...:linha}}` | {{linguagem e runtime, camadas, acesso a dados, autenticação, testes}} |
| Front-end | {{Design System ou template em `base-frontend/`, ou "não se aplica: o projeto não tem interface"}} | `{{base-frontend/...}}` | {{componentes, layouts, padrões}} |

- **Linguagem e runtime:** {{conforme a referência, ou "a confirmar"}}
- **Arquitetura:** {{conforme a referência, ou "a confirmar"}}
- **Banco de dados previsto:** {{resposta ou "a confirmar"}}
- **Hospedagem e ambientes:** {{resposta ou "a confirmar"}}
- **Comandos de build, teste e execução:** {{os que a referência documenta, ou "a confirmar"}}
- **Exceções à referência:** {{lista com o motivo, ou "nenhuma"}}
- **Origem da escolha:** a stack vem das bases de referência acima (obrigatórias); divergência delas só como exceção registrada.

## Integrações externas

| Integração | Sentido | Decisão | Fonte |
|---|---|---|---|
| {{serviço}} | {{recebe / envia / ambos}} | {{interface fixa / pode mudar / a confirmar}} | {{resposta / documento}} |

## Restrições

- {{o que não pode mudar ou faltar: prazo legal, contrato, idioma, acessibilidade, desempenho, volume de usuários, dispositivos}} — {{fonte}}

## Regras confirmadas

| # | Regra | Fonte |
|---|---|---|
| R1 | {{regra de negócio, com exemplo numérico quando houver cálculo}} | {{Resposta do usuário (AAAA-MM-DD) / documento, seção}} |

## Roadmap

| # | Plano | O que é | Situação |
|---|---|---|---|
| 01 | {{módulo}} | {{uma frase}} | ⬜ a planejar |

## Riscos e perguntas em aberto

- {{risco ou dúvida}} — {{origem}}

**Quem decide as regras de negócio:** {{nome ou papel, ou "a confirmar"}}
