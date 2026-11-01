# Guia — Domínio

Objetivo: levantar as entidades, as regras de negócio e onde elas moram hoje, e propor bounded contexts e aggregates para a migração. Leia STACK.md e ARQUITETURA.md antes.

**A proposta respeita o banco atual.** As tabelas não mudam na migração. O aggregate usa o tipo de chave atual da tabela (numérico, texto, UUID, chave composta). Não proponha outro tipo de chave.

## O que ler

1. **Entidades e tabelas.** Classes ou estruturas que o projeto mapeia para o banco (pelo ORM, por anotações, por arquivos de mapeamento ou por classes de modelo ligadas a SQL manual) e os scripts SQL. Para cada entidade: tabela, tipo da chave, relacionamentos.
2. **Enums e constantes de negócio.** Enums, classes de constantes, status guardados como `string` ou `int` mágico.
3. **Regras de negócio.** Procure onde as decisões acontecem:
   - Métodos em entidades (domínio rico) ou só propriedades (domínio anêmico).
   - Services de aplicação.
   - Controllers e views (regra no lugar errado).
   - Stored procedures, triggers e views do banco, se houver scripts no repositório.
   - Validações (`if` que lança exceção ou retorna erro, bibliotecas de validação, anotações, regras no formulário).
   Cite cada regra com `caminho:linha` e diga em que camada ela está.
4. **Regras implícitas.** Comportamento que parece regra de negócio, mas cujo motivo o código não explica: números mágicos, `if` de status, cálculos de datas, filtros fixos. Marque com ❓. Elas viram perguntas para o usuário.
5. **Linguagem ubíqua.** Termos do negócio nos nomes de classes, tabelas, telas e mensagens. Registre sinônimos (dois nomes para a mesma coisa) e homônimos (um nome para coisas diferentes).
6. **Bounded contexts.** Agrupe entidades que mudam juntas e que compartilham linguagem. Use como pistas: pastas, módulos, controllers ou rotas, prefixos de tabela e menus.
7. **Aggregates.** Para cada contexto, proponha aggregates: raiz, entidades internas, value objects candidatos, invariantes que a raiz protege. Justifique com as regras levantadas. Referências entre aggregates são por identificador.
8. **Eventos de domínio candidatos.** Ações que disparam efeitos em outro lugar (envio de e-mail, notificação, atualização de outro módulo).

## Modelo

Copie a partir daqui para `.planning/migracao/DOMINIO.md`.

---

# Domínio atual

Unidade mapeada: `{{caminho}}` · Gerado em {{data}}

## Resumo

- Entidades: {{n}} · Tabelas: {{n}} · Enums: {{n}}
- Estilo do modelo: {{anêmico / rico / misto}} — evidência: `{{caminho:linha}}`
- Onde moram as regras: {{controllers / services / entidades / banco}}

## Entidades

| Entidade | Tabela | Chave (tipo) | Relacionamentos | Evidência |
|---|---|---|---|---|
| {{Entidade}} | {{tabela}} | {{Id (tipo)}} | {{1:N Outra}} | `{{caminho:linha}}` |

## Enums e constantes de negócio

| Nome | Valores | Persistido como | Evidência |
|---|---|---|---|
| {{Status}} | {{Ativo, Inativo}} | {{int / string}} | `{{caminho:linha}}` |

## Regras de negócio

| # | Regra | Camada atual | Evidência |
|---|---|---|---|
| R1 | {{descrição}} | {{controller / service / entidade / SP}} | `{{caminho:linha}}` |

## Regras implícitas ❓

| # | O que o código faz | Dúvida | Evidência |
|---|---|---|---|
| I1 | {{comportamento}} | ❓ {{por que / ainda vale?}} | `{{caminho:linha}}` |

## Linguagem ubíqua

| Termo | Significado no código | Sinônimos / conflitos | Evidência |
|---|---|---|---|
| {{termo}} | {{significado}} | {{outros nomes}} | `{{caminho:linha}}` |

## Bounded contexts propostos

| Contexto | Entidades | Pistas no código | Dependências de outros contextos |
|---|---|---|---|
| {{Contexto}} | {{entidades}} | `{{pasta / módulo / controller}}` | {{contextos}} |

```mermaid
graph LR
  {{ContextoA}} --> {{ContextoB}}
```

## Aggregates propostos

### {{Contexto}} — {{Aggregate}}

- Raiz: `{{Entidade}}` (chave `{{tipo atual}}`) — `{{caminho:linha}}`
- Entidades internas: {{lista}}
- Value objects candidatos: {{CPF, Endereço, Período...}} — `{{caminho:linha}}`
- Invariantes: {{R1, R3}}
- Referências a outros aggregates (por Id): {{lista}}
- Esforço para extrair: {{P/M/G}}

## Eventos de domínio candidatos

| Evento | Disparado por | Efeito atual | Evidência |
|---|---|---|---|
| {{ResidenteAdmitido}} | {{ação}} | {{e-mail, atualização}} | `{{caminho:linha}}` |

## Perguntas para o usuário

- {{pergunta que o código não responde}}
