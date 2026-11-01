# Referências

O PEV é uma aplicação própria de uma prática conhecida: **Spec-Driven Development (SDD)**, em que a especificação (e não o código) é o artefato principal, e o código é gerado e validado contra ela. Estas são as ferramentas e os materiais que apareceram na pesquisa de 2026-11-01. As fontes são secundárias (blogs e listagens): confira nos sites oficiais antes de citar.

## Ferramentas de SDD

| Ferramenta | O que é | O que há de parecido com o PEV |
|---|---|---|
| **GitHub Spec Kit** | Kit de código aberto, que funciona com vários agentes de IA. Ciclo: especificar → planejar → tarefas → implementar. | O ciclo e a ideia de a especificação mandar no código |
| **Kiro (AWS)** | IDE própria (um VS Code modificado) com o passo de especificação embutido. Paga. | O planejamento em etapas |
| **OpenSpec** | Ferramenta de linha de comando baseada em mudanças, pensada para código que já existe. | O foco em evoluir sistemas existentes |
| **BMAD-METHOD** | Método com vários agentes, mais pesado, que vai além de software. | A divisão de papéis |
| **Superpowers (obra)** | Plugin do Claude Code com skills para cada fase: brainstorming gera a especificação, planos geram a lista de tarefas, e um subagente executa cada tarefa com uma revisão em seguida. | O mais próximo do PEV |

## O que o PEV tem de próprio

- Adaptado ao uso do Claude Code com **Opus para planejar e revisar e Sonnet para implementar** (`opusplan`).
- **Postulados do projeto** na revisão: o revisor confere as regras do `CLAUDE.md`, não só a qualidade genérica do código.
- **Migração de legado como fluxo próprio:** mapeamento, matriz de cobertura, testes de paridade e carga de dados.
- **O plano como memória entre sessões**, com status por etapa, para retomar numa sessão limpa.
- **História de usuário opcional** como entrada, com o questionário aplicado só nos pontos nebulosos.

## Links

- [7 Spec-Driven Development Tools: Spec Kit, Kiro, OpenSpec, Tessl & More](https://ssojet.com/blog/best-spec-driven-development-tools)
- [Spec Kit vs BMAD vs OpenSpec: choosing an SDD framework in 2026](https://dev.to/willtorber/spec-kit-vs-bmad-vs-openspec-choosing-an-sdd-framework-in-2026-d3j)
- [Spec-Driven Frameworks Compared: Spec Kit vs OpenSpec vs BMAD vs Superpowers](https://developertoolkit.ai/en/ecosystem/frameworks/spec-driven-frameworks/)
- [Comprehensive Guide to Spec-Driven Development: Kiro, GitHub Spec Kit, and BMAD-METHOD](https://medium.com/@visrow/comprehensive-guide-to-spec-driven-development-kiro-github-spec-kit-and-bmad-method-5d28ff61b9b1)
- [DataCamp: Spec-Driven Development with Claude Code](https://www.datacamp.com/de/tutorial/spec-driven-development-with-claude-code)
- [Subagent-driven development (Superpowers)](https://skillselion.com/skills/jnmetacode/superpowers/subagent-driven-development)
