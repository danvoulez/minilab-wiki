# Guia de transição — do cockpit antigo ao monitor `minilab.work`

Este guia descreve como levar a UI atual (já profissional) ao novo app, sem
jogar fora o que funciona. A estrutura visual permanece; o que muda é a
**função**: de painel de controle para **monitor de observabilidade e
emergência**. Cada seção diz o que estava, o que passa a ser, e por quê.

---

## 0. A inversão, em uma frase

O app velho abre cada espaço com *"Talk to agent"* e uma pilha de quick
actions — ele pergunta **"o que você quer fazer?"**. O app novo abre com
**estado** — ele responde, antes de você perguntar, **"isto aqui está
quebrado, e é por isto."**

Observabilidade é a prioridade primária. Ação é secundária e excepcional —
há caminhos mais simples pra agir por fora. Toda decisão de design abaixo
deriva dessa única inversão.

---

## 1. A invenção central — estado como propriedade física

É a mudança que carrega o app inteiro. No velho, o estado é texto que você
precisa **ler** (`WORKERS 4/4`, `HEARTBEAT 12s`). No novo, o estado é uma
propriedade **física** do quadro: você o **vê** sem ler, como vê que um
paciente está pálido antes de medir qualquer coisa.

São quatro estados, lidos por **três canais redundantes ao mesmo tempo**.
A redundância não é enfeite: é o que faz o app funcionar para daltônicos e
sob luz ruim às 4h da manhã — o momento em que ele mais importa.

| Estado        | Preenchimento | Saturação        | Movimento                    |
|---------------|---------------|------------------|------------------------------|
| **Saudável**  | cor cheia, topo a base | plena   | respiro lento (~7s), quase imperceptível |
| **Degradado** | ~55%, recua | lavada / dessaturada | rotação dos sinais **congela** no problema |
| **Caído**     | ~12%, quase vazio | cinza        | silêncio                     |
| **SOS / morto** | cinza       | cinza            | **pulso âmbar** — o único movimento que grita |

Três leis governam isso:

**Movimento é o recurso mais caro da tela — gaste pouco.** Saúde é calma:
o card saudável só respira devagar pra provar que está vivo, não congelado.
Quanto pior o estado, mais a tela se mexe. Pânico = movimento; nominal =
quietude. Se três coisas pulsam, nada pulsa — o pulso é exclusivo do SOS.

**O nível se move devagar e com histerese.** A altura da cor transita em
~1–2s, nunca salta. E para trocar de humor (saudável ↔ degradado) o valor
precisa cruzar o limiar **e ficar lá** alguns segundos. Sem isso, métrica
oscilando em torno de 50% vira um caça-níquel epilético — pior que texto.

**Dado vivo se prova com movimento.** Um número parado pode ser telemetria
fresca ou um app travado há uma hora mostrando o último valor. Por isso o
heartbeat conta (`12s → 13s → 14s`), a fila pulsa, o cabo flui. Movimento
sutil e contínuo = a tela provando que o dado é real. Para observabilidade,
isso é credibilidade, não enfeite.

---

## 2. O grid (cockpit) — o que muda

A grade de quadrados coloridos permanece — é forte e você gosta dela. Três
mudanças:

**Renomear e re-semantizar os quadrados.** Mantém-se cor e posição; troca-se
o significado:

| Antigo            | Novo          | Categoria       | Papel real                                   |
|-------------------|---------------|-----------------|----------------------------------------------|
| LAB 512 (compute) | **FORCE 512** | COMPUTE         | inferência local; workers, fila, execução    |
| LAB 8GB (antenna) | **WORK 8GB**  | ANTENNA         | gestão das empresas geradas pelo Build; relay/ingress |
| LAB 256 (cockpit) | **BUILD 256** | COCKPIT         | gera novos eve; controle, dev, coordenação   |
| SUPABASE          | **NEON**      | RECORD          | banco das empresas (Vercel/Neon)             |
| —  (novo)         | **CLOUDFLARE**| EDGE · DNS      | domínios, DNS, endpoints sem compute         |
| LAB ID            | **LAB ID**    | IDENTITY        | credenciais, capacidades, confiança          |
| GOOGLE / DRIVE    | **G. DRIVE**  | BACKUP          | preservação, arquivos, bundles               |
| APPS              | **APPS**      | CATALOG         | ferramentas e consoles do operador           |
| FLOWS             | **FLOWS**     | ORCHESTRATION   | runs coordenados, automações, aprovações     |

(SETTINGS sai da grade e vira a engrenagem no header — é utilitário, não tem
agente.)

**O quadrado vira sinal vital.** A gramática da seção 1 se aplica a cada
tile: saudável é cor cheia e respirando, degradado recua e desbota, morto
pulsa âmbar. A grade inteira passa a ser legível de relance.

**Duas coisas novas que o velho não tinha:**

- *Faixa de emergência no topo.* Hoje o header diz só `9 ativos`. Agora,
  com tudo verde, ela encolhe para uma linha discreta ("todos os sistemas
  nominais"). Quando algo quebra, vira uma faixa vermelha com **ticker**
  correndo a causa já resolvida — *"FORCE 512 · degradado · fila 27 jobs
  travada · enlace→WORK ok"*. É o que transforma "abri no susto" em "sei o
  que olhar em 2 segundos". O ticker horizontal mora só aqui, na faixa
  larga; no card pequeno ele seria ilegível.

- *O cabo FORCE↔WORK como aresta viva.* O enlace ethernet é infra crítica e
  silenciosa — quando cai, derruba inferência→apps sem avisar. Ele vira uma
  linha animada entre os dois cards: fluxo correndo = tráfego saudável;
  fluxo lento = degradando; linha parada e vermelha = morto. E quando morre,
  **os dois cards entram em atenção juntos**, porque a dependência agora é
  visível em vez de implícita.

**A rotação que congela.** Dentro de cada card de máquina, um chip mostra um
check de prontidão por vez (`conexão ok → integridade ok → atividade ok`),
ciclando devagar quando saudável. No instante em que algo degrada, a rotação
**para** e fixa o check problemático, em âmbar. É a regra: *rotação é um luxo
do estado saudável; problema cancela a rotação e fixa o foco.* Mostrar 1 de N
é ótimo quando está tudo bem — mas nunca pode esconder a única coisa que
quebrou.

---

## 3. A página de detalhe — fixa, quatro zonas

Princípio inegociável: **a página não rola.** Tudo que importa cabe num
relance. O que rola é a *grade* (a lista de espaços) e *componentes dentro de
caixas de altura fixa* (a row de lentes rola na horizontal). Uma página que
rola esconde estado abaixo da dobra — num app de emergência, isso é falha de
segurança.

A ordem das zonas resolve dois eixos ao mesmo tempo:

- **Eixo do olho (cima → baixo):** veredito → aprofundar → porquê → referência.
- **Eixo do polegar (baixo → cima):** a ação-estrela fica ao alcance; o dado
  que você só lê é o piso.

```
┌──────────────────────────────────┐
│ ‹  FORCE 512   COMPUTE      [ ! ] │  1 · IDENTIDADE + veredito
├──────────────────────────────────┤
│  ████ card de estado (menor)     │  2 · VEREDITO
│  ⚠ fila travada · enlace ok      │     cor/pulso = "está quebrado?"
├──────────────────────────────────┤
│ [Workers][Fila][Enlace][Logs] →  │  3 · LENTES (scroll horizontal)
│                                  │     cada chip lê OU age
├──────────────────────────────────┤
│  ▸  Perguntar ao agente          │  4 · PORQUÊ (estrela, zona do polegar)
│     por que este estado?         │
├──────────────────────────────────┤
│  Fila       27 jobs  ▓▓▓▓░       │  5 · PISO (sinais vitais)
│  Workers    4 / 4                │     leitura pura, sempre visível
│  Enlace→WORK  direto             │
└──────────────────────────────────┘
```

**Zona 1 — Identidade.** Voltar, nome, categoria, pílula de estado. Quem é,
qual o veredito.

**Zona 2 — Card de estado (menor que antes).** O card vivo da seção 1, em
versão compacta. Ele pode encolher justamente porque a cor responde "está
quebrado?" instantaneamente — não precisa ser grande, precisa ser
inconfundível. A linha de causa aparece aqui quando não está saudável.

**Zona 3 — Row de lentes (rola na horizontal).** É a fusão das antigas quick
actions com a observabilidade. Cada chip é uma **lente** sobre uma dimensão:
`Workers`, `Fila`, `Enlace`, `Logs`, `Ingress`. Alguns só abrem uma vista
focada (observabilidade pura); outros disparam algo (`Retentar`, `Drenar nó`)
e aí ganham a seta e, se forem de risco, a cor vermelha. Mesma faixa, tudo
deslizando lado a lado — a página nunca se mexe.

**Zona 4 — Agente (botão grande, zona do polegar).** Permanece como estrela,
mas com papel invertido: ele **explica o estado**, não recebe ordens. "Por
que FORCE está vermelho?" → ele lê os sinais daquele escopo e responde a
causa. É leitura, é seguro, e é onde um agente escopado brilha. Fica baixo e
grande porque é o gesto mais frequente no momento de pânico.

**Zona 5 — Sinais vitais (o piso).** A lista persistente de prontidão +
sinais, com as mini-barras de perigo (`27 jobs ▓▓▓▓░`). É leitura pura — você
varre com os olhos, não toca — então é o rodapé sempre-visível. Para o
operador experiente que dispensa o agente, é aqui que ele lê tudo de uma vez.

---

## 4. Um agente por espaço — ainda vale, com uma condição

Vale, desde que **cada agente seja literalmente o observador daquela
superfície**, e não um chat genérico. A condição é o escopo:

- **Contexto de sessão escopado:** o agente do FORCE só sabe de inferência;
  o do WORK só de empresas/relay; o do BUILD só de geração de eve. Cada
  instância recebe um `system context` com qual máquina é, quais sinais lê,
  quais ações pode disparar.
- **Toolset restrito = as lentes da página.** As tools do agente são as
  mesmas lentes/ações daquele espaço, nada além. Force não fala de DNS;
  Cloudflare não dá `drain node`.
- **Papel de observador, não operador.** O default é explicar anomalia. Ação
  existe como exceção confirmada.

O template eve da Vercel encaixa direto: é a mesma UI de agente recebendo
contexto e ferramentas mais estreitos por espaço. Sem esse escopo, você teria
9 agentes fazendo a mesma coisa — aí a ideia desanda. Espaços sem telemetria
própria (SETTINGS) não precisam de agente.

---

## 5. A verdade dos dados — real vs. placeholder

O ponto mais importante e o menos visual. Toda a UI de observabilidade
depende de existir **algo emitindo estado das três máquinas**. No protótipo,
os números são telemetria simulada (o heartbeat conta, a fila sobe) só para
provar a gramática. Antes de ligar em produção:

1. **Cada eve precisa reportar.** Um heartbeat + métricas de cada máquina
   (FORCE: fila, workers, latência do enlace; WORK: ingress, empresas no ar,
   enlace; BUILD: eve ativos, divergência de repo) escrevendo num lugar que a
   UI lê. O **Neon** serve bem como esse destino.
2. **Defina o baseline.** `Fila 3 jobs` só significa algo se a UI sabe que o
   normal é 0–5 e que 40 é incêndio. Sem baseline, todo número é ruído. Os
   limiares das mini-barras e das transições de estado saem daqui.
3. **Só então a UI.** A interface lê estado + baseline e aplica a gramática.
   Sem os passos 1 e 2, o app fica bonito e mente — e a política tem que ser
   *placeholder permitido, verdade falsa proibida* (o velho já dizia isso na
   tela do Supabase; mantenha).

---

## 6. Ordem de construção sugerida

Do código atual até o app novo, na ordem que entrega valor mais cedo:

1. **Gramática de estado nos tiles.** Trocar o card de cor-fixa pelo card de
   preenchimento + saturação + respiro/pulso. É a mudança que mais transforma
   a percepção, e é puramente de front.
2. **Faixa de emergência + ticker.** O maior ganho para o caso "abri no
   susto". Pode começar lendo os mesmos placeholders.
3. **Página de detalhe fixa de 4 zonas.** Reorganizar o detalhe existente na
   hierarquia da seção 3; cortar as quick actions verticais para a row de
   lentes horizontal.
4. **Telemetria real (seção 5).** Pôr os eve reportando para o Neon e trocar
   o placeholder pelo dado vivo. A partir daqui a gramática vira verdade.
5. **O cabo vivo.** Com FORCE e WORK reportando o enlace, animar a aresta e
   a atenção conjunta.
6. **Agentes escopados.** Encaixar o template eve por espaço com contexto e
   tools estreitos, no papel de observador.

A regra geral: a UI (passos 1–3) pode andar antes do dado, porque demonstra e
valida a linguagem; mas ela só vira observabilidade de verdade quando os eve
estiverem reportando (passo 4). Construa a língua primeiro, ligue a verdade
em seguida.
