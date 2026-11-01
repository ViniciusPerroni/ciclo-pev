# História — <Título curto>

**Tipo:** <Módulo novo | Evolução de módulo existente | Acabamento (vale para várias telas)>
**Módulo / área:** <ex.: Financeiro, Cadastros › Clientes>
**Pedido por / data:** <nome, AAAA-MM-DD>
**Status:** <Rascunho | Pronta para planejar | Planejada (Plano NN)>

<!--
COMO USAR ESTE MODELO
- Preencha o que souber e deixe o resto em branco ou como "a decidir". O planejamento (skill pev-planejar) usa o que estiver escrito e faz perguntas só sobre o que faltar.
- O mínimo para planejar: seções 1 (Contexto), 2 (Objetivo), 3 (Quem usa), 4 (Histórias) e 11 (Fora do escopo). O resto deixa o plano mais preciso e as perguntas mais curtas.
- Exemplos concretos valem mais que descrições: "entrou em 10/10 e o dia de cobrança é 05: a primeira fatura cobra de 10/10 a 04/11". Um caso com números e datas evita o plano interpretar do jeito errado.
- Apague os comentários como este quando terminar.
-->

## 1. Contexto e motivação  *(mínimo)*

<Por que isso é necessário agora. Como o usuário resolve hoje, sem o sistema ou com a versão atual. Qual problema ou oportunidade está por trás.>

## 2. Objetivo  *(mínimo)*

Ao fim, <quem> consegue <o quê>:
- <resultado 1, em linguagem de usuário>;
- <resultado 2>.

<!-- Em evolução de módulo, diga também o que NÃO muda para o usuário. -->

## 3. Quem usa  *(mínimo)*

| Perfil | O que faz | O que não pode ver ou fazer | Perfil novo? |
|---|---|---|---|
| <ex.: Gerente> | <ações> | <ex.: não vê os dados de outra unidade> | Não |

<!-- Se o sistema guarda dado sensível (LGPD, sigilo), diga o que cada perfil NÃO deve ver. Perfis de administração da plataforma, se existirem, não precisam entrar aqui. Se um perfil faz o papel de outro (ex.: o gerente também faz o papel do financeiro), escreva. -->

## 4. Histórias de usuário  *(mínimo)*

### H1 — <título>

**Como** <perfil>, **quero** <ação>, **para** <benefício>.

**Fluxo** (o caso real, passo a passo):
1. <o que o usuário faz>
2. <o que o sistema responde>
3. <…>

**Critérios de aceitação** (use um exemplo com números ou datas):
- **CA1.1** *Dado* <situação>, *quando* <ação>, *então* <resultado esperado>.
- **CA1.2** …

**Regras ligadas:** <números da seção 6, ex.: R2, R5>.

### H2 — <título>

**Como** … **quero** … **para** …

**Fluxo:** …

**Critérios de aceitação:**
- **CA2.1** …

<!-- Uma história por objetivo do usuário, não por tela. Numere H1, H2…: o plano cita esses números para provar que nada ficou de fora. -->

## 5. Conceitos e estados

**Conceitos** (os termos do módulo, em negrito, como a equipe fala):
- **<Termo>:** <o que é>.

**Situações** (os estados pelos quais um registro passa):

| Situação | Quem muda | Para onde vai | Volta? |
|---|---|---|---|
| <ex.: Rascunho> | <perfil> | <Emitida, descartada> | <Não> |

**Correção de erros:** <como se corrige um registro errado: anulação ou cancelamento com motivo, descarte de rascunho. Lembre que registro emitido (financeiro, contábil, clínico) normalmente não se apaga.>

## 6. Regras de negócio

| # | Regra | Exemplo concreto | Situação |
|---|---|---|---|
| R1 | <a regra, ex.: a mensalidade é proporcional aos dias do ciclo> | <ex.: 26 de 31 dias × R$ 3.100,00 = R$ 2.600,00> | Decidida |
| R2 | <regra que ainda não está clara> | | A decidir |

<!-- "Decidida" = o plano usa como está. "A decidir" = o plano pergunta, com uma proposta. Cálculos, prazos, limites, validações e permissões entram aqui. -->

## 7. Telas, relatórios e avisos

**Telas:**

| Tela | Quem usa | O que mostra ou faz | Parecida com (tela-modelo do projeto, se souber) |
|---|---|---|---|
| <Tela> | <perfil> | <…> | <ex.: a lista de faturas> |

**Relatórios:** <nome, filtros, quem pode, se tem impressão. Ou "nenhum".>

**Avisos e notificações:** <quando avisar, quem recebe, quando o aviso some. Ou "nenhum".>

**Impressões e documentos:** <ex.: fatura impressa no layout de invoice. Ou "nenhum".>

## 8. Integração com o que já existe

| O que já existe | Como se relaciona | Muda? |
|---|---|---|
| <módulo, tela, dado ou regra> | <usa, estende, respeita> | <Não / Sim: o quê> |

### Antes e depois  *(só em evolução)*

| Hoje | Depois |
|---|---|
| <como funciona agora> | <como deve ficar> |

**Registros que já existem:** <o que acontece com o que já foi gravado: ganha um valor padrão, é recalculado, não é afetado, precisa de uma carga. Ou "não se aplica".>

**Telas, regras e perfis que mudam:** <lista, para a documentação e os testes não esquecerem nada.>

## 9. Dados e massa de teste

**Dados novos importantes:** <campos e de onde vêm.>

**Dados de teste (seed) esperados:** <quantidade e variedade para a tela e os relatórios ficarem realistas, ex.: "3 meses de faturas, com pagas, em atraso e canceladas". Ou "a definir no plano".>

## 10. Pontos a decidir

<!-- O que você ainda não sabe. Se tiver uma ideia, escreva-a como proposta; o planejamento transforma cada ponto numa pergunta objetiva. -->

- [ ] **P1.** <pergunta> *Proposta:* <sua ideia, se tiver>.
- [ ] **P2.** …

## 11. Fora do escopo  *(mínimo)*

- <o que não entra desta vez, e se for o caso, por quê ou em que momento entra>.

## 12. Documentação

<Telas ou regras que mudam para o usuário (a documentação do usuário acompanha toda mudança) e as perguntas que ele provavelmente vai fazer. Pode ficar "a definir no plano".>

## 13. Referências

- **Legado:** <arquivos do sistema legado ligados ao tema, se houver>.
- **Planos e decisões relacionados:** <ex.: Plano 10 (D-98), Plano 17 (CRM)>.
- **Visual:** <telas-modelo, protótipos, imagens>.
- **Externas:** <normas, legislação, conversas, documentos>.
