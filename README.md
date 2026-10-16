# FICHA DE DECISÕES · War Room FiapBank (CP6 · 3 aulas)

> Este arquivo é o **README.md do repositório do grupo** (`cp6-warroom-<>`).
> Vale **5,0 pontos** (rodadas 0,5 · relâmpagos 0,3), e a nota é pela
> **justificativa**, não pela letra. Preencham após cada aula e commitem até
> **23h59 do mesmo dia** (regras completas na seção 5 do enunciado).
>
> **Os incidentes da madrugada são revelados só em aula.** O título de cada registro
> será **ditado pelo professor na hora**; ninguém se antecipe.

**Grupo (nome da equipe plantonista):** OS RESENHAS

**Turma:** 2CCPX **Repo:** `cp6-warroom-grupo38`

**Integrantes (nome + RM):**

| Nome | RM |
|------|----|
|  Fernando Melo |  RM564297 |
|  Patrick Mansour | RM562970  |
|  Pedro Henrique Ribeiro | RM565090  |
|  Pietro Mauer | RM564345  |
|  Ryan Santos | RM565102  |
|  Samir Assad | RM561562  |

## 0. Setup do repositório (antes da 1ª aula; podem apagar esta seção depois)

1. Um integrante cria o repo **público** no GitHub: `cp6-warroom-<nome-do-grupo>`
   (ex.: `cp6-warroom-debugadores`), com um README qualquer
2. Substituam o conteúdo do `README.md` por este template (no navegador, pelo próprio
   GitHub, ou clonando):
   ```bash
   git clone https://github.com/<conta>/cp6-warroom-<nome-do-grupo>.git
   cd cp6-warroom-<nome-do-grupo>
   # substitua o conteúdo do README.md por este template e:
   git add .
   git commit -m "chore: ficha em branco do grupo"
   git push
   ```
3. Postem o **link no Teams** (o mesmo link para todo o grupo)

**Convenção de commits** (1 commit por rodada; relâmpagos podem ir junto com a
rodada seguinte):

```
decisao: R1 - opcao C (<resumo da justificativa em uma frase>)
decisao: R2 - opcao A (<resumo em uma frase>)
pos-mortem: relatorio de incidente da madrugada
```

---

## 📁 Dossiê técnico do FiapBank (MVP em produção)

**Stack:** Java 17 + Spring Boot + Spring Data JPA + Oracle. API com endpoints em
`/api/contas` e `/api/transferencias` (cenário visto desde a Aula 13).

**Contrato e regras de negócio que o banco prometeu aos clientes e aos reguladores:**

| Regra | Como deve ser |
|---|---|
| Transferência **PIX** | taxa **R$ 0,00** |
| Transferência **TED** | taxa fixa **R$ 5,00** |
| Saldo | **nunca fica negativo**: transferência/saque sem saldo é recusado com erro claro |
| Número de conta | **sequencial e único** (1001, 1002, 1003...), gerado pelo sistema |
| Extrato de transferência | grava **quem pagou** e **quem recebeu**, na ordem certa |
| CPF | **dado sensível**: nunca aparece nas respostas da API |
| Consultas ao banco | sempre parametrizadas, e cada operação **usa e libera** a conexão |
| Suíte de testes | roda antes de todo deploy; **verde** é pré-requisito pra subir |

**Como escrever a justificativa:** nomeie o **mecanismo técnico** em jogo (o
conceito das Aulas 11 a 15 que explica o incidente) e o **trade-off** (velocidade ×
segurança × faturamento × dívida). "Porque é mais seguro" não é justificativa.

---
# 📝 REGISTRO DE DECISÕES

## Rodada 1: O saldo que virou negativo

**Tipo:** rodada · **Voto:** D

**Justificativa:**
Optamos pelo rollback da versão para garantir estabilidade imediata e rápida recuperação do serviço antes da Black Friday. O trade-off técnico foi o descarte temporário de correções recentes da semana, aceitando uma leve degradação na reputação em troca de mitigar o prejuízo financeiro iminente.

**Placar do grupo após esta decisão:** 🔥 6 · 💰 R$ 15 mil · 🧹 3

---

## Rodada relâmpago A: A busca que não acha ninguém

**Tipo:** relâmpago · **Voto:** B

**Justificativa:**
Aproveitamos a infraestrutura e os recursos nativos do Spring Data JPA com a *derived query* `findByTitular` pronta no repositório. O trade-off arquitetural elimina o filtro manual por referências ineficientes em memória, padronizando a consulta e limpando a dívida técnica acumulada.

**Placar do grupo após esta decisão:** 🔥 7 · 💰 R$ 15 mil · 🧹 2

---

## Rodada 3: A noite das contas gêmeas

**Tipo:** rodada · **Voto:** B

**Justificativa:**
Optamos por gerar números de contas por valores randômicos/UUID para solucionar rapidamente as colisões de concorrência sem travar o contador global. O trade-off foi contornar a rigidez do Singleton sequencial em prol de uma solução ágil de contorno, gerando um leve acréscimo na dívida técnica de auditoria dos extratos.

**Placar do grupo após esta decisão:** 🔥 7 · 💰 R$ 15 mil · 🧹 3

---

## Rodada 4: O comprovante que aponta pra pessoa errada

**Tipo:** rodada · **Voto:** B

**Justificativa:**
Escolhemos corrigir o erro de inversão de parâmetros na entidade e reprocessar integralmente o histórico de extratos afetados. O trade-off investiu mais tempo de processamento de madrugada para assegurar a integridade absoluta dos dados regulatórios e financeiros.

**Placar do grupo após esta decisão:** 🔥 8 · 💰 R$ 40 mil · 🧹 2

---

## Rodada 5: A fábrica que devolve null

**Tipo:** rodada · **Voto:** A

**Justificativa:**
Implementamos o padrão *fail-fast* diretamente na Factory de contas, lançando uma exceção explícita para tipos inválidos em vez de retornar nulos. O trade-off conteve o erro na raiz (na própria fábrica), impedindo que exceções tardias e silenciadas poluíssem o fluxo de persistência no repositório.

**Placar do grupo após esta decisão:** 🔥 8 · 💰 R$ 40 mil · 🧹 2

---

## Rodada relâmpago C: O cadastro com metade dos campos

**Tipo:** relâmpago · **Voto:** B

**Justificativa:**
Adotamos o padrão de projeto *Builder* para centralizar os valores padrão e as regras de validação no método `build()`. O trade-off evitou o crescimento descontrolado de construtores sobrecarregados, mantendo o código limpo e coeso sem gerar novos débitos técnicos de nulos espalhados.

**Placar do grupo após esta decisão:** 🔥 8 · 💰 R$ 40 mil · 🧹 2

---

## Rodada 6: O teste vermelho às 03h45

**Tipo:** rodada · **Voto:** B

**Justificativa:**
Decidimos investir tempo na investigação do teste que falhou, mesmo correndo o risco de estourar a janela de congelamento. O trade-off priorizou a descoberta da causa raiz do contrato de taxas em relação à pressa cega de subir código sem validação confiável.

**Placar do grupo após esta decisão:** 🔥 7 · 💰 R$ 70 mil · 🧹 2

---

## Rodada 7: Go/No-Go da Black Friday

**Tipo:** rodada · **Voto:** B

**Justificativa:**
Optamos por atrasar ligeiramente a abertura da campanha para corrigir os desvios encontrados pela suíte de testes e consolidar a estabilidade. O trade-off trocou algumas horas iniciais de pico por um ambiente robusto, seguro e livre de cobranças indevidas de taxas aos clientes.

**Placar do grupo após esta decisão:** 🔥 8 · 💰 R$ 170 mil · 🧹 1

---

## 🔎 O caminho do MEU grupo

| # | Decisão | Nossa letra | Consequência que ELA teria tido |
|---|---|---|---|
| R1 | O saldo que virou negativo | D | Rollback rápido, perda de correções anteriores da semana |
| RA | A busca que não acha ninguém | B | Adoção de *derived query* do Spring Data JPA, zerando a dívida de busca |
| R3 | A noite das contas gêmeas | B | Geração por aleatoriedade, agilizando colisões mas gerando ruído na auditoria |
| R4 | O comprovante que aponta pra pessoa errada | B | Correção da gravação e reprocessamento histórico, garantindo integridade |
| R5 | A fábrica que devolve null | A | Aplicação de *fail-fast* na Factory, isolando o erro na origem |
| RC | O cadastro com metade dos campos | B | Uso de *Builder* para concentrar regras de defaults com segurança |
| R6 | O teste vermelho às 03h45 | B | Investigação da falha antes da janela, descobrindo inconsistências reais |
| R7 | Go/No-Go da Black Friday | B | Abertura sólida com atraso planejado, blindando o banco de prejuízos maiores |

---

# 📋 PÓS-MORTEM · relatório de incidente

## 1. Linha do tempo da madrugada
A madrugada começou às 19h com instabilidades críticas de saldo causadas por exceções engolidas em blocos `catch` silenciosos. Passamos por refatorações estruturais de persistência (substituindo concorrência manual por *derived queries* e *Builders*), corrigimos inversões em extratos e tratamos inconsistências em factories. Fechamos a madrugada investigando falhas em suítes de testes críticos e optando por um atraso estratégico controlado na abertura da Black Friday para garantir a integridade dos dados e a confiança dos usuários.

## 2. Causa raiz de 2 incidentes

**Incidente 1:** O saldo que virou negativo (Rodada 1)
* **Aula/mecanismo:** Tratamento inadequado de exceções (*Exception Swallowing* / Exceções engolidas em blocos `try-catch`). O fluxo interno capturava o erro de saldo insuficiente mas prosseguia com a operação de débito na entidade de origem.

**Incidente 2:** A fábrica que devolve null (Rodada 5)
* **Aula/mecanismo:** Violação do princípio *Fail-Fast* em padrões de criação (*Factory Pattern* com retorno nulo implícito). O método retornava `null` para tipos desconhecidos, espalhando o erro para camadas distantes de persistência e gerando falhas de `NullPointerException` difíceis de rastrear.

## 3. O que faríamos diferente (2 rodadas + por quê)

1. **Rodada 1 (Voto C em vez de D):** Deveríamos ter corrigido o método e adicionado o teste unitário (`assertThrows`) logo no início em vez de apelar para o Rollback geral, o que evitou a perda de correções já validadas da semana.
2. **Rodada 3 (Voto D em vez de B):** Em vez de adotar identificadores aleatórios, teríamos delegado a geração sequencial diretamente ao banco de dados via `sequence`, garantindo a unicidade regulatória perante o Bacen desde o princípio sem gerar dívidas de auditoria.

## 4. A maior lição da equipe
A pressa em produção cobra um preço altíssimo quando o código não possui uma rede de testes automatizados honesta e uma arquitetura desacoplada. Decisões baseadas em atalhos rápidos geram um efeito cascata de dívida técnica nas rodadas seguintes, provando que parar para estruturar o código corretamente salva horas preciosas de crise em pleno plantão.

* **Palavra-chave da madrugada:** **Resiliência**
_______________________________________________________________________________________
