# Tabuleiro — Especificação

> Runtime de colaboração humano–LLM sobre um estado compartilhado, verificável e legível por terceiros.
>
> Este é o documento único e resolvido. Ele carrega a tese, a arquitetura e o racional inteiros. Onde toca identidade, hashing, lifecycle ou schema dos objetos, ele **defere** aos quatro documentos normativos `BOARD_DECISIONS_v0.1`, `BOARD_OBJECTS`, `BOARD_LIFECYCLE` e `BOARD_VERTICAL_SLICE`, que regulam esses pontos. `Tabuleiro` / `The Board` são o mesmo sistema; codinome trocável.
>
> **Vocabulário sanitizado para a Dream Machine:** neste documento, o Envelope/Board usa
> `Proposal`, `BoardAct`, `Confirmation`, `Sealing` e `BoardCommit`. Os nomes antigos
> `Candidate`, `Act` nu, `admission` e `admitted` ficam proibidos sem qualificador de
> jurisdição. Um `BoardAct` é cânone interno do Envelope; não é um LogLine Act nem uma
> consequência soberana.

---

## 0. Como ler este documento — precedência normativa

Existem três camadas, e elas se empilham em vez de competir:

- **Camada conceitual e arquitetural — este documento.** Governa a tese, os planos, o racional, o modelo de ameaça, as Scenes, a autoridade dinâmica e tudo o que os BOARD docs não cobrem. É o front-door: quem chega lê isto primeiro.
- **Camada normativa — os quatro BOARD docs.** Governam a identidade dos objetos, a canonização, o cálculo de hash, a ordem determinística de criação, as máquinas de estado e as tabelas da fatia vertical. São a fonte de verdade contra a qual o código é escrito.
- **Casca de entrega — o plano de PR.** Layout de arquivos, lista de testes, critérios de aceitação.

**Regra de conflito:** onde este documento e um BOARD doc divergirem sobre identidade, hashing, lifecycle ou schema, **o BOARD doc vence**. Este documento nunca redefine um hash body; ele descreve a intenção e aponta para o regulamento. Se você for implementar, a identidade vem do BOARD doc; o porquê vem daqui.

### 0.1 Invariantes congelados (carga estrutural — intocáveis por passadas de estilo)

Estes são os invariantes que carregam peso. Qualquer reescrita futura, por mais bonita que seja, que viole um destes está errada por construção. Cada um é regulado pelo documento indicado.

1. **Identidade acíclica.** A identidade de um `Shift` **nunca** inclui o hash do objeto que ele produz. O objeto produzido cita o shift; um `ShiftResult` separado amarra shift→output depois que ambos os hashes existem. Nada de hash provisório. — *normativo: BOARD_DECISIONS §1–3, BOARD_OBJECTS §7–8.*
2. **Relógio de arming reseta na edição.** Marcar uma `Proposal` como `ready` inicia o arming sobre aquela versão exata. Qualquer edição material depois disso volta a Proposal para `editing` e zera o arming. O `input_hash` do Confirmation Shift é o `proposal_version_hash` exato confirmado. — *normativo: BOARD_DECISIONS §6, BOARD_LIFECYCLE §4.*
3. **Confirmação não prova compreensão.** Prova que a versão exata esteve disponível para revisão, que o ator tinha autoridade, que o arming decorreu, que o ator assumiu responsabilidade, e que o BoardAct selado é rastreável àquele Shift. — *normativo: BOARD_DECISIONS §7.*
4. **Projeção é válida por prefixo.** Uma Projection `as_of_seq = N` tem que bater com a lista ordenada de BoardAct hashes `1..N`, a menos que se declare `partial` com `selection_reason`. Proibido escolher atos a dedo em silêncio. — *normativo: BOARD_DECISIONS §10, BOARD_OBJECTS §10.3.*
5. **Transição consequente de Finding é BoardAct ou Shift.** Nenhuma transição de Finding que bloqueie confirmação, restaure autoridade, dispense violação, resolva gap ou mude risco pode acontecer por mutação silenciosa. — *normativo: BOARD_DECISIONS §8.*
6. **Canonização travada por stream.** Cada stream declara `canonicalization` e `hash_algorithm` na criação e nunca os muda no meio da cadeia. Hoje `board-json-v0`; alvo de interoperabilidade JCS/RFC 8785 é um stream novo, não uma troca in-place. — *normativo: BOARD_DECISIONS §11, BOARD_OBJECTS §1.*
7. **BoardAct é append-only e encadeado.** `seq` monotônico por stream, `prev_board_act_hash` aponta ao anterior (ou genesis). Correção é supersessão por elo para frente; nunca deleção, nunca edição. — *normativo: BOARD_OBJECTS §6.*
8. **Digest antes da evaporação.** O `source_digest` é computado antes que qualquer Event da janela evapore, e o campo `zone` fica fora do corpo do digest. — *normativo: BOARD_OBJECTS §4.2, BOARD_LIFECYCLE §2.3.*
9. **Todo output tem recibo.** Toda Proposal, BoardAct, Finding e Projection produzida tem um `ShiftResult` (ou entra num output-set manifest). — *normativo: BOARD_OBJECTS §7–8.*
10. **Atenção em shadow-mode no v0.1.** Os arming delays são por tier de risco; a anomalia de atenção emite Finding mas ainda não rebaixa autoridade na primeira fatia. — *normativo: BOARD_LIFECYCLE §5.3.*

---

## 1. Tese

O chat convencional é um substrato fraco para trabalho sério entre humano e IA. Numa conversa normal, o mundo é reconstruído turno a turno: o estado da tarefa vive dentro de texto, memória, suposição e boa-fé. O modelo pode afirmar que algo aconteceu quando não aconteceu. O humano pode aprovar sem ter lido. E quem chega depois não tem como distinguir ruído de intenção, intenção de decisão, decisão de execução, execução de correção, correção de prova.

O Tabuleiro inverte esse padrão por uma decisão estrutural: **a conversa é descartável, o estado é permanente, o registro é verificável, e a projeção é legível por quem não estava lá.** Nada vira verdade por ter sido dito. Algo entra no cânone interno do Envelope só depois de ser condensado, revisado, confirmado com fricção deliberada e selado num ledger append-only. O modelo propõe; o humano confirma; o ledger prova; o Board reconcilia; a projeção explica.

O efeito é uma colaboração em que os dois lados são empurrados para a honestidade. O modelo não consegue alucinar estado com facilidade, porque o estado está visível e hasheável — uma afirmação que não virou BoardAct simplesmente não é cânone do Envelope. E o humano não consegue carimbar de leve, porque o próprio ato de confirmar é medido, cronometrado e auditado. A verdade interna do Board passa a custar fricção, dos dois lados da mesa.

Quatro unidades fundam tudo. A unidade de percepção não é o prompt; é a **cena** — uma projeção limitada e proposital do estado (§10). A unidade de agência não é o agente; é o **shift** — toda operação com entrada, ator, duração e resultado fechado. A unidade de identidade não é o nome; é o **hash** do conteúdo canônico. E a unidade de verdade interna do Envelope não é a afirmação; é o **BoardAct selado** — confirmado com fricção, encadeado, superseedível mas nunca apagável.

---

## 2. Os três planos

**O fast loop** contém eventos brutos: gente falando, modelo respondendo, ferramenta emitindo saída, agente operando. É alto-volume, ruidoso e temporário. Serve à percepção, não à verdade. Os eventos atravessam zonas de vida — `live` → `buffered` → `evaporated` — com política padrão de 90s vivo, até 600s em buffer, depois evaporado. Quando evaporam, o texto bruto pode sumir; o que sobrevive deles é o digest (§7).

**A membrana** é onde a atividade bruta vira possibilidade estruturada. Ela lê uma janela limitada de eventos e produz uma `Proposal`. É aqui que o LLM normalmente trabalha. A membrana não cria consequência — cria proposta. Sua frase é: "dado o que acabou de acontecer, eis o BoardAct que talvez mereça confirmação e sealing". Antes que a janela evapore, ela computa o `source_digest`, que mais tarde permite provar se uma janela apresentada é a mesma que gerou a Proposal.

**O slow loop** é o ledger permanente: BoardActs selados, ligados por hash e número de sequência, append-only. É o estado durável e compartilhado da colaboração dentro do Envelope, e é o que um terceiro audita. Ele não precisa reter a conversa bruta inteira — precisa dos BoardActs selados, seus hashes, sua proveniência e sua relação com os anteriores.

A reconciliação entre o que foi dito/observado (semântico) e o que foi confirmado/feito (cinemático) é um **derivado** dos dois loops, materializado no Board (§8), não um quarto plano.

---

## 3. Os objetos canônicos

Os schemas exatos e os corpos de identidade de cada objeto são **regulados por `BOARD_OBJECTS`**. O que segue é a intenção de cada um, na ordem em que importam.

**Event** — atividade crua no fast loop. Responde "o que foi dito ou observado?", mas não responde "o que foi aceito como verdade?". Efêmero. O `zone` muda com o tempo e por isso fica fora do corpo do digest. (`BOARD_OBJECTS §4`.)

**Proposal** — proposta estruturada extraída de uma ou mais janelas de eventos, produzida pela membrana via Condensation Shift. Não é verdade nem consequência; é rascunho de BoardAct esperando revisão. Carrega slots — `who`, `what`, `scope`, `object`, `cancellations`, `expects`, `risk` — mais `confidence` (autoavaliação do modelo, não autoridade). É **mutável** enquanto vive, e por isso sua identidade é um `proposal_version_hash`, não verdade permanente. Marcar `ready` arma a versão exata; editar depois de ready desarma e versiona de novo (invariante 2). (`BOARD_OBJECTS §5`, `BOARD_LIFECYCLE §4`.)

**BoardAct** — a unidade atômica de cânone interno do Envelope. Imutável depois de selado, append-only, com `seq` e `prev_board_act_hash`. Responde "o que foi oficialmente confirmado no estado compartilhado do Board?". Correções não editam BoardActs — superseedem (§7). O `board_act_hash` é computado sobre o corpo de identidade sem o próprio campo de hash. (`BOARD_OBJECTS §6`.)

**Shift** — a unidade de agência. Qualquer operação com entrada, ator, duração e caminho de resultado fechado é um Shift: condensar, confirmar, investigar, projetar, verificar, efetuar. Responde "quem ou o quê agiu, sobre qual entrada, produzindo qual saída, sob quais condições?". É a decisão de design mais importante depois da tese: o sistema não audita só decisões — audita **o trabalho que produziu** as decisões. A evidência (incl. latência de atenção) anexa ao Shift, não ao BoardAct. (`BOARD_OBJECTS §7`.)

**ShiftResult** — o recibo imutável que amarra um Shift fechado ao output que ele produziu, gravado *depois* que o hash do output existe. É a peça que torna a identidade acíclica (§4). Todo output produzido tem um. (`BOARD_OBJECTS §8`.)

**Finding** — discrepância detectada entre o esperado e o confirmado, ou entre o processo exigido e o observado. Responde "o que não reconcilia?". É o conteúdo vivo do Board. Resolução fica fora da identidade do Finding. (`BOARD_OBJECTS §9`.)

**Projection** — visão legível do ledger para uma audiência. Não é transcrição nem chat bruto: é renderização narrativa de BoardActs selados, Findings abertos, supersessões, riscos e gaps. Válida por prefixo (invariante 4). (`BOARD_OBJECTS §10`.)

Dois objetos de apoio importam ao racional, embora não sejam top-level: **BoundExpectation** — obrigação futura criada por um BoardAct, que vira gap se vencer sem cumprimento (§8) — e **Scene** — a projeção limitada que um Shift de modelo tem permissão de ler (§10).

---

## 4. Identidade acíclica e o ShiftResult

Esta é a resolução técnica mais difícil do sistema, e ela é **normativa em `BOARD_DECISIONS §1–3`**. Vale entender o porquê, porque é fácil reintroduzir o defeito sem perceber.

Se a identidade do Shift incluísse o hash do que ele produz, surgiria uma circularidade: a `Proposal` cita `proposed_by = condensation_shift_hash`; o `BoardAct` cita `provenance.confirmation_shift = confirmation_shift_hash`; a `Projection` cita `built_by = projection_shift_hash`. Em todos os três, o objeto depende do shift. Se o shift, por sua vez, dependesse do hash do objeto (via `output_hash` na identidade, ou via evidência que contenha o hash do output), o hash de um passaria a depender do hash do outro — rabo mordendo a cabeça. A "saída" ingênua é hash provisório atualizado depois, e isso destrói a imutabilidade, que era o ponto.

A resolução: **o `output_hash` é removido da identidade do Shift.** O shift prova *que houve agência*; o `ShiftResult` prova *o que a agência produziu*, e é gravado depois que os dois hashes já existem. O padrão canônico, em quatro tempos: *Shift primeiro. O objeto produzido cita o Shift. O hash do objeto é computado. O ShiftResult amarra o Shift ao objeto.* O grafo de identidade fica acíclico por construção, e nenhum hash é atualizado depois de criado. Atenção, ao implementar a confirmação: o `board_act_hash` **não** pode entrar na evidência do Confirmation Shift, sob pena de ressuscitar a circularidade.

---

## 5. O ciclo de vida

O fluxo padrão — eventos no fast loop → membrana condensa em Proposal → humano revisa e edita → Proposal é armada → Confirmation Shift sela como BoardAct → BoardAct é anexado ao ledger → o Board checa drift/gaps/riscos → uma Projection renderiza o estado — é **regulado em detalhe por `BOARD_LIFECYCLE`**, incluindo as máquinas de estado de Event e Proposal e a ordem determinística de criação de cada objeto.

O princípio que atravessa todo o ciclo: o modelo não atualiza o mundo diretamente; o humano não muta o registro em silêncio; o ledger não depende de memória; o Board não confia só na conversa. Os estados de Proposal (`pending` → `editing` → `ready` → `board_committed`, com saídas `rejected`/`withdrawn`/`expired`) e a regra de que só `ready` confirma e sela, com o arming preso à versão exata, estão em `BOARD_LIFECYCLE §4`.

---

## 6. O portão de verdade

A confirmação não é um clique de UI — é um `Shift` de `kind: confirmation`, e o que ela prova está congelado no invariante 3: disponibilidade da versão exata, autoridade suficiente, decurso do arming, aceitação de responsabilidade, rastreabilidade do BoardAct àquele Shift. **Não** prova compreensão — e essa honestidade sobre o próprio limite é o que mantém o sistema íntegro.

A fricção é deliberada e proporcional ao risco. Os arming delays padrão — L0/L1 5s, L2 10s, L3 20s, L4 60s — não existem para deixar o trabalho lento; existem para tornar a aprovação cega *visível e difícil de fazer por reflexo*. A latência entre abrir e confirmar entra como evidência no Shift; confirmações repetidas abaixo do limiar fisiológico emitem `Finding{kind:"attention_anomaly"}`. No v0.1 isso é shadow-mode: registra, ainda não pune (invariante 10). O cálculo exato de `can_confirm` e a forma final do `commit_proposal` estão em `BOARD_LIFECYCLE §5`.

A defesa contra o ataque óbvio — marcar ready, esperar o relógio, editar a substância e confirmar na hora — é o reset de arming na edição (invariante 2). Sem ele, o portão de atenção seria contornável; com ele, você só confirma exatamente a versão que ficou exposta pelo tempo de arming inteiro.

---

## 7. Proveniência, efemeridade e supersessão

O fast loop pode sumir, mas a prova de origem permanece. Antes da evaporação, a membrana computa `source_digest` sobre a janela canônica de eventos (sem o `zone`). Depois, um auditor talvez não recupere o texto original — mas, se alguém apresentar a janela original, ele a hasheia e compara com o digest guardado. O sistema separa duas perguntas: "ainda dá para ler a conversa bruta?" (talvez não) e "dá para verificar que este BoardAct veio daquela janela exata?" (sim, se a janela for apresentada). É o equilíbrio entre privacidade, retenção e auditabilidade.

A integridade do ledger vem do encadeamento: cada BoardAct aponta ao anterior por hash, num log linear estilo Merkle. Adulterar um BoardAct no meio quebra todos os hashes seguintes — é o que o comando de verificação detecta. E correção nunca é reescrita: um BoardAct novo supersede os atingidos por elo **para frente** (`superseded_by`), o BoardAct velho permanece, e a projeção explica "isto foi aceito antes no Board; foi superseedido por este BoardAct posterior; eis o porquê". A imutabilidade do passado é uma propriedade, não uma promessa.

---

## 8. Conformidade e reconciliação — o Board

O Board não é o ledger; é a superfície de reconciliação. Ele mostra os Findings não resolvidos: gaps, anomalias, violações, passos fora de ordem, passos faltando, extras inesperados, fechamento prematuro, descompasso de risco. O ponto é estrutural: o sistema **não** depende de um humano reparar a divergência na conversa — ele a detecta comparando estruturas canônicas, que é o que o chat em linguagem natural não consegue fazer.

Um `Workflow` é uma máquina de estado sobre BoardActs selados: quais stages existem, quais transições são permitidas, o que é obrigatório, se loops são aceitos, qual autoridade um passo arriscado exige. Uma `Expectation` é uma obrigação futura criada por um BoardAct — `start_engine` cria a expectativa de `verify_engine` até um prazo. Se o BoardAct esperado não chega no prazo, o Board cria um `Finding{kind:"gap"}`; se chega depois, o Finding é resolvido e aponta ao BoardAct que o cumpriu. O gap deixa de ser ausência invisível e vira objeto de primeira classe, com identidade, referências, severidade e caminho de resolução. A distinção `pending_expectation` (esperado mas não atrasado) vs. `gap` (vencido ou renderizado como trabalho aberto) está em `BOARD_OBJECTS §3.3`; a regra de que transição consequente de Finding exige BoardAct/Shift é o invariante 5.

---

## 9. Autoridade dinâmica

Autoridade não é só papel — é propriedade viva. Um ator tem um papel-base (`Curator`, `Confirmer`, `Investigator`, `Marshal`, `Archivist`, `Reader`) com um teto de risco, mas a autoridade *efetiva* depende do comportamento observado: `autoridade efetiva = teto do papel ajustado pelo integrity_score`. Confirmações limpas e deliberadas sobem o score; anomalias de atenção, violações e descuido descem. Quando o score cai, o teto efetivo cai junto — quem aprova ações arriscadas sem evidência de atenção perde o direito de confirmar BoardActs de alto risco até reconquistar confiança. É a ideia mais forte da camada de governança: o mesmo sistema que mede honestidade **usa** a medida para ajustar permissão. As portas externas só abrem se `autoridade efetiva ≥ risco requerido`. No v0.1 a parte de rebaixamento é shadow-mode (invariante 10); o enforcement vem depois.

---

## 10. Scenes — o que o LLM tem permissão de ver

O LLM não lê o mundo inteiro. Para cada Shift, o sistema monta uma **Scene**: uma projeção limitada e proposital do estado relevante àquela operação. Uma Scene pode conter os BoardActs relevantes, os Findings abertos, informação de workflow, as instruções do assento atual, janelas de evento limitadas e as restrições de risco. O modelo opera a partir da Scene, não de acesso irrestrito ao ledger ou ao histórico bruto. Isso protege privacidade, reduz sobrecarga de contexto e torna o comportamento do modelo mais fácil de auditar — e devolve a "cena como unidade de percepção" ao seu lugar operacional: o que o modelo percebe é sempre uma construção limitada, hasheável, e não a totalidade.

---

## 11. Os assentos do LLM

O modelo tem assentos legítimos, todos como Shifts auditáveis, todos roteados por um gateway, todos entregando `output_hash` que o portão confirma ou recusa — nunca verdade direta. **Condensação:** lê janelas de evento e propõe Proposals. **Investigação:** lê Findings e propõe explicações, hipóteses ou resoluções. **Projeção:** transforma ledger e Board em narrativa legível por audiência. **Proposta de efeito:** em casos de alto risco, propõe efeitos externos, que exigem confirmação humana autorizada. A regra é constante: o LLM propõe, o protocolo registra, o portão humano confirma, o ledger prova.

---

## 12. Efeitos externos

Alguns BoardActs podem descrever efeitos no mundo — mandar email, chamar API, mudar um ticket, fazer deploy, controlar um dispositivo. Isso passa por `Effect Shifts` explícitos e nunca é efeito colateral escondido de um output de modelo. Um efeito externo exige um BoardAct selado, uma porta/ferramenta definida, um nível de risco requerido, um ator autorizado e um Shift auditável. Quanto maior o risco, mais forte a exigência de confirmação. Na Dream Machine completa, efeitos perigosos ou irreversíveis ainda precisam atravessar a membrana e obter autoridade LogLine. No v0.1, efeitos externos estão fora de escopo (ver `BOARD_VERTICAL_SLICE §2`).

---

## 13. Projeção para terceiros

A Projection responde à pergunta central da tese: alguém que não estava na conversa consegue entender o que aconteceu? Ela mostra o que foi confirmado no Board, o que continua aberto, o que foi superseedido, que riscos havia, que gaps existem, e que evidência liga o estado ao processo. Audiências diferentes pedem projeções diferentes: o `operator` precisa do Board ativo; o `newcomer` precisa do resumo narrativo; o `auditor` precisa de hashes, proveniência e verificação de cadeia. A Projection não é conveniência de UI — é artefato central do protocolo, e é válida por prefixo (invariante 4): ela não pode contar uma história conveniente omitindo atos.

É o grafismo da F1 sobreposto ao rádio: a fala entre piloto e equipe só faz sentido para eles porque referencia um estado que vive na cabeça dos dois; a projeção é o tabuleiro que torna esse estado legível para quem só ouviu o rádio.

---

## 14. Replay e time-travel

Como os BoardActs são append-only e sequenciados, o estado em qualquer ponto passado é reconstruível: `state(as_of_seq = N)` aplica os atos `seq ≤ N` respeitando as supersessões vigentes naquele ponto. Isso habilita estado num ponto, diff entre dois pontos, reconstrução forense, projeções históricas e prova do que se sabia no momento da decisão. Importa porque muitas disputas não são sobre o que é verdade agora — são sobre o que era conhecido, confirmado ou pendente quando uma decisão foi tomada.

---

## 15. Modelo de ameaça

O sistema é desenhado em torno de falhas reais, e assume que nem humano nem modelo são perfeitos. **O modelo pode alucinar** — mitigação: ele só propõe Proposals, humanos confirmam, digests preservam proveniência. **O humano pode aprovar cego** — mitigação: arming delay, latência de atenção, detecção de anomalia, autoridade dinâmica. **O banco pode ser adulterado** — mitigação: BoardActs encadeados, ferramenta de verificação, checkpoints externos. **A história pode ser reescrita** — mitigação: ledger append-only e supersessão em vez de edição. **A autoridade pode ser abusada** — mitigação: integrity score comportamental, tetos de papel, limiares de risco, confirmação reforçada em alto risco.

---

## 16. Fatia vertical

A primeira implementação prova o loop inteiro com a menor superfície possível: um stream de eventos com TTL; um **Condensation Shift real** (LLM de verdade na membrana — este é o soquete cuja ausência invalidaria a prova); revisão e edição de Proposal; confirmação com arming delay e evidência de latência; um ledger de BoardActs append-only e encadeado; um WorkflowMatcher simples; um Board com ao menos `missing` e `gap`; uma Projection read-only para `newcomer`; e um comando de verificação que detecta adulteração. As tabelas SQLite, os endpoints e o cenário de demonstração (`start_engine → verify_engine → taxi`) são **regulados por `BOARD_VERTICAL_SLICE`**.

O critério de sucesso é único e falsificável: **um terceiro abre a Projection, entende o que aconteceu sem ter visto o fast loop, vê o que está pendente, e verifica que a projeção corresponde ao estado confirmado na cadeia de hashes.** Os dois testes que selam isso são o que prova que a projeção exclui o payload bruto do fast loop e o que prova que adulterar um BoardAct quebra a verificação. Quando ambos passam *e* a projeção saiu de uma condensação real, a tese deixou de ser tese.

---

## 17. Questões em aberto

- **Modelo de turno.** Este documento assume "humano confirma, modelo propõe". O canvas multi-peça — lances dos dois lados sobre um estado visível em tempo real — é a maior decisão de produto ainda não tomada, e merece passada própria. Turno toca dados, primitivas, papéis e UI ao mesmo tempo.
- **Concorrência multi-ator** no mesmo stream: last-write-wins, lock por região, ou CRDT no fast loop.
- **Granularidade da peça.** Quão discreto um BoardAct deve ser para "mover uma peça" significar algo preciso sem engessar trabalho criativo.
- **Calibração do limiar de atenção** — risco real de falso positivo punindo um usuário rápido e honesto; por isso shadow-mode primeiro.
- **A metade experiencial.** Esta fatia prova a verificabilidade (a cadeia, o portão, a projeção). A tela comum viva — o tabuleiro visível que os dois manipulam — é a outra metade da tese e fica para depois. Provar a coluna vertebral não é provar o produto.

---

*A conversa evapora. A Proposal espera. O humano confirma. O BoardAct entra na cadeia. A projeção explica.*
*O modelo propõe; o portão confirma; a cadeia prova; o Board reconcilia; a projeção traduz.*
