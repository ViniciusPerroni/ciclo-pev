# Guia — Qualidade

Objetivo: listar os débitos técnicos com evidência e o peso de cada um na migração. Leia INVENTARIO.md, STACK.md, ARQUITETURA.md e DOMINIO.md antes.

**Você não roda comandos.** Contagens feitas com Grep (`output_mode: count`) são medidas: cite o padrão. Cobertura, complexidade ciclomática e acoplamento ficam "não medido", com o comando ou a ferramenta que mediria nesta stack.

Não assuma a stack: descubra no projeto a convenção de teste, o jeito de tratar erros e as ferramentas de análise, e escreva os padrões de Grep para a linguagem que o projeto usa. Teste cada padrão numa amostra antes de citar a contagem.

## O que ler

1. **Testes.**
   - Módulos ou pastas de teste, o framework de testes, as bibliotecas de mock e de asserção.
   - Quantidade de testes: descubra a convenção que marca um teste na stack (anotação, prefixo de nome, função de declaração) e conte com Grep nos arquivos de teste. Cite o padrão.
   - O que é testado: domínio, serviços, controllers, integração com banco.
   - Cobertura: "não medido". Indique o comando ou a ferramenta que mediria.
2. **God Classes.** Use os 20 maiores arquivos de INVENTARIO.md. Para cada um acima de 500 linhas: o que faz, quantas responsabilidades, quem depende dele.
3. **Smells com Grep.** Para cada categoria, escreva o padrão adequado à linguagem do projeto, registre a contagem e até 3 exemplos com `caminho:linha`:
   - Mau uso de concorrência (espera síncrona sobre código assíncrono, tarefas sem controle)
   - Captura genérica de erro (pega qualquer exceção ou erro)
   - Erro engolido (tratamento vazio ou que descarta o erro)
   - SQL montado por concatenação ou interpolação (risco de SQL injection)
   - Estado global mutável
   - Data e hora lidas do relógio local, espalhadas pelo código (difícil de testar, problema com fuso)
   - Marcadores: `TODO|FIXME|HACK|XXX`
   - Código comentado em bloco
4. **Segurança.**
   - Segredos em arquivo versionado: nome do arquivo e da chave. Nunca o valor.
   - Endpoints ou rotas sem autenticação ou autorização que deveriam ter.
   - Validação de entrada ausente nas camadas de entrada.
   - Dados sensíveis (saúde, CPF, documentos) em log.
5. **Rigor da linguagem e análise estática.** Modo estrito ou checagem de tipos desligados, avisos que não quebram o build, supressões de aviso ou de lint no código, ferramentas de lint e análise estática configuradas.
6. **Dependências.** Dependências obsoletas ou sem suporte na versão atual da plataforma (citadas em STACK.md). Versões divergentes da mesma dependência.
7. **Duplicação visível.** Blocos ou classes repetidos entre módulos. Cite os pares. Não estime percentual.

## Severidade

- **Crítico:** bloqueia a migração ou é risco de segurança ou de perda de dados.
- **Alto:** aumenta muito o esforço ou o risco da migração.
- **Médio:** atrapalha, mas pode ser resolvido durante a migração.
- **Baixo:** cosmético.

## Modelo

Copie a partir daqui para `.planning/migracao/QUALIDADE.md`.

---

# Qualidade atual

Unidade mapeada: `{{caminho}}` · Gerado em {{data}}

## Resumo

- Testes: {{n}} em {{n}} módulos · Cobertura: não medido
- God Classes (> 500 linhas): {{n}}
- Débitos críticos: {{n}} · altos: {{n}}

## Testes

| Módulo de teste | Framework | Testes | O que cobre | Evidência |
|---|---|---|---|---|
| {{módulo}} | {{framework}} | {{n}} | {{domínio / serviços}} | `{{caminho:linha}}` |

Padrão de contagem: `{{regex}}`
Cobertura: não medido — `{{comando ou ferramenta que mediria}}`

## God Classes

| Arquivo | Linhas | Responsabilidades | Dependentes | Esforço |
|---|---|---|---|---|
| `{{caminho}}` | {{n}} | {{lista}} | {{n ou "não verificado"}} | {{P/M/G}} |

## Smells

| Smell | Padrão | Ocorrências | Exemplos |
|---|---|---|---|
| {{categoria}} | `{{regex}}` | {{n}} | `{{caminho:linha}}` |

## Segurança

| Achado | Severidade | Evidência |
|---|---|---|
| {{segredo em arquivo de configuração versionado, chave X}} | {{crítico}} | `{{caminho:linha}}` |

## Rigor da linguagem e análise estática

| Item | Situação | Evidência |
|---|---|---|
| Modo estrito / checagem de tipos | {{ligado / desligado / ausente}} | `{{caminho:linha}}` |
| Avisos tratados como erro | {{...}} | `{{caminho:linha}}` |
| Lint / análise estática | {{ferramenta ou "nenhuma"}} | `{{caminho:linha}}` |

## Métricas não medidas

| Métrica | Ferramenta ou comando |
|---|---|
| Cobertura | {{comando ou ferramenta da stack}} |
| Complexidade ciclomática | {{ferramenta da stack}} |
| Acoplamento | {{ferramenta da stack}} |

## Débitos técnicos

| # | Débito | Severidade | Impacto na migração | Esforço | Evidência |
|---|---|---|---|---|---|
| D1 | {{descrição}} | {{crítico}} | {{impacto}} | {{P/M/G}} | `{{caminho:linha}}` |

## Perguntas para o usuário

- {{pergunta que o código não responde}}
