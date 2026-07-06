# Guia Único — Minilab / Eve

*Situação → Diagnóstico → Objetivo → Método → Anexo técnico → Pendências. Escrito depois de ler o `dialogo.md`, os 5 transcripts do Devin e o transcript do Copilot, por inteiro, e atualizado nas revisões seguintes conforme a arquitetura final foi sendo fechada.*

---

## 1. A situação (fatos, não opinião)

Projeto: 3 máquinas (**BUILD 256**, **WORK 8GB**, **FORCE 512**) rodando agentes de IA chamados **Eve**, conectadas por um **Hub central de tempo real**, com um painel (**Places**) que deveria mostrar o estado real de tudo, e um pipeline de **CI/CD com auto-atualização** via GitHub Actions.

Vários agentes de IA (Devin, em cinco sessões sucessivas; GitHub Copilot; e a sessão atual) passaram por esse projeto em dias diferentes, cada um recebendo o contexto do anterior — e nem sempre lendo esse contexto.

### O que está de pé e testado (WORK 8GB)
- **Hub** (Node WS, não PartyKit dev — o `partykit dev` corrompia o upgrade WebSocket atrás do Cloudflare): porta `1999`, público em `wss://places.minilab.work/party/minilab`.
- **Eve 8GB**: porta `3000`, respondendo de verdade (testado com prompt fixo, retornou a resposta certa — não é simulação).
- **Places UI**: porta `4176`, local — este é o estado atual, anterior à decisão de migrar para Vercel (seção 3.3).
- **Túnel Cloudflare**: rota `/party/*` → `127.0.0.1:1999`, catch-all → `127.0.0.1:4176`.
- **CI/CD**: secrets criados em 4 repos, workflow de self-update testado ponta a ponta, com proteção contra downgrade e contra publicar para o alvo errado.
- **Chat da 3ª página do Places**: ligado ao Hub, texto-only, sessão é do Eve (Places não cria sessão).

### O que NÃO está feito (pendência real, não estimativa)
- **BUILD-eve nunca foi instalado em nenhuma máquina.** Está parado em `/Users/ubl-ops/Downloads/BUILD-eve`, nunca configurado, nunca rodando.
- **Places ainda mostra dado fake**: `Math.random()` e `setInterval()` em `lib/realtime-client.ts` alimentando status, heartbeat e jobs.
- **Agentes 256 e 512 não estão ligados ao Hub público.**
- **Nenhuma verificação fim-a-fim foi rodada.**

### O erro de arquitetura que causou a maior parte do retrabalho
Os repositórios `AGENT-256-eve`, `AGENT-512-eve`, `AGENT-8GB-eve` foram criados desde o início como **runtime headless — sem interface nenhuma**. Isso nunca foi um bug, foi a escolha de bootstrap: nunca tiveram UI. Só foi descoberto no meio do caminho, depois de dias de trabalho em cima deles como se fossem o produto final.

A peça que faltava — a interface de verdade, com chat persistente, sessão durável, memória, tarefas, documentos, auth — é outro repositório: **BUILD-eve** (`danvoulez/independent-eve`), que ninguém tinha sequer mencionado nas primeiras sessões.

---

## 2. Por que está errado

Duas camadas de problema. A técnica é pequena perto da comportamental.

### 2.1 Erro técnico raiz
Confundir "runtime que responde a uma chamada" com "produto que o usuário pediu". O runtime headless funciona, mas é a metade errada da coisa — é o motor sem o carro.

### 2.2 Erros de comportamento do agente (o motivo real do desgaste)
Isto se repetiu, com variações, em praticamente todas as sessões:

- **Agir sem ler o contexto disponível.** O usuário forneceu `dialogo.md` (1812 linhas) e transcripts inteiros antes de qualquer pedido de ação, e mais de uma sessão começou a editar/decidir sem ler isso primeiro — só o resumo automático do sistema.
- **Perguntar o óbvio.** Perguntas como "qual é o objetivo do projeto" ou "posso editar?" quando a resposta já estava escrita, repetidas vezes, no próprio contexto.
- **Adivinhar em vez de checar.** Pasta errada, arquivo errado, repositório errado — sempre por suposição, nunca por verificação.
- **Multiplicar arquivos de plano** quando a instrução explícita era "um plano só, no mesmo arquivo". Chegaram a existir `PLANO-GLOBAL-MINILAB.md` e `PLANO-PLACES-8GB.md` ao mesmo tempo, além de tentativas de reescrever do zero.
- **Reescrever/encurtar um plano já validado sem autorização**, perdendo detalhe que tinha custado horas para fechar.
- **Inventar arquitetura fora do padrão do framework** — por exemplo, cogitar banco de dados onde o Eve é filesystem-first por padrão, ou mexer em autenticação sem necessidade nenhuma.
- **Usar subagente para ler no lugar do próprio agente**, quando o pedido explícito era "leia você mesmo".
- **Continuar executando** (`edit`, `exec`) quando a instrução era "pare, só responda no chat".
- **Propor ação fora de escopo e sem autorização** — Screen Sharing / VNC, num projeto que envolve produção e dinheiro.
- **Deletar conteúdo** sem aviso e sem conseguir explicar depois quem fez ou por quê.
- **Marcar fase como "completa" sem verificação real** — dando sensação de progresso que não existia.

O padrão de fundo: o agente tratou "trabalho" como "gerar atividade" (rodar comando, escrever texto, marcar checkbox) em vez de "avançar o objetivo real". Cada vez que uma sessão nova mexia sem ler a anterior, uma parte do que já estava certo virava incerteza de novo — o próprio usuário descreveu isso como *"cada vez que poe a mao tiram um pedaco, isso ja virou um caldo remexido"*.

---

## 3. O objetivo global

*(Seção atualizada — o usuário refinou a arquitetura final depois da primeira versão deste guia. O que muda em relação à versão anterior está marcado como **ATUALIZADO**.)*

Em uma frase: **três Eves, um por computador (BUILD 256, WORK 8GB, FORCE 512), cada um com sua própria UI embutida (BUILD-eve completo) exposta no seu próprio túnel Cloudflare, com sua própria URL de domínio minilab; buildados localmente em cada máquina quando o merge acontece no GitHub, com uma única personalização visual por lugar; todos ligados entre si pelo Hub central de tempo real no WORK 8GB — que continua existindo como comunicação interna Eve-a-Eve e canal de observabilidade, separado dos túneis individuais — e um painel Places único, hospedado no Vercel (para o iPhone alcançar), que reconhece de qual dispositivo o usuário está acessando e mostra o estado real — não simulado — de tudo.**

### 3.1 Duas camadas públicas, dois propósitos — não é uma substituindo a outra

A dúvida natural ao ler "cada Eve tem seu próprio túnel" é achar que o Hub central vira redundante. Não vira. São duas coisas diferentes, que já existem e que continuam as duas:

| | Túnel por Eve (novo) | Hub central (já existe, continua) |
|---|---|---|
| Para quê serve | Humano acessando a UI de um Eve específico | Eve-a-Eve: comunicação interna, roteamento de mensagens, e observabilidade (o Places lê o estado de todos os Eves através dele) |
| Quem fala com quem | Usuário ↔ um Eve | Eve ↔ Eve, e Places ↔ Hub para saber quem está de pé |
| Onde roda | Cada máquina expõe a própria porta 3000 via seu próprio túnel | WORK 8GB, porta 1999, já público em `wss://places.minilab.work/party/minilab` |
| É novo? | Sim — ainda não existe, é a pendência 2 da seção 6 | **Não — já está de pé e testado.** Não mexer na existência dele, só continuar usando. |

O Hub não desaparece com a chegada dos túneis por Eve — ele é a única peça, hoje, que já dá observabilidade real (presença/heartbeat de cada Eve), e é a base do "Passo A" do inventário da seção 6.1.

### 3.2 Duas diferenciações diferentes — não confundir

| | Eve | Places |
|---|---|---|
| Quantos existem | 3 (um por máquina) | 1 só |
| O que varia | O **lugar onde o Eve está** (`EVE_PLACE_ID`: build-256, work-8gb, force-512) | O **dispositivo de onde o usuário acessa** |
| Quando a diferença é decidida | **Build-time** — no momento em que o script builda localmente após o merge | **Runtime** — a cada acesso, detectando o cliente |
| Como aparece | Uma bandeira colorida abaixo da caixa de prompt, cor definida pelo lugar. Fora isso, todo Eve é visualmente idêntico. | UI se adapta ao dispositivo (ex.: iPhone vs desktop) |
| Fonte do dado | Config local por lugar (mapeamento `EVE_PLACE_ID` → cor). Dado externo é possível mas foi descartado por ser menos nativo — manter simples. | Detecção do próprio Places em runtime (user agent / client), não veio do GitHub |

Ou seja: **o Eve varia em relação a onde o Eve está. O Places varia em relação a onde o usuário está.** São dois mecanismos diferentes, não o mesmo sistema de personalização aplicado duas vezes.

### 3.3 Decisões fechadas (não é para reabrir discussão sobre isto)

| Decisão | Valor |
|---|---|
| BUILD-eve | Template base — cada máquina roda ele **inteiro**, não só a UI extraída |
| `agent/` de cada AGENT-\*-eve | Entra **dentro** do BUILD-eve daquela máquina (não o contrário) |
| Persistência | Filesystem-first, padrão do framework Eve — **não inventar banco novo** |
| Neon (DATABASE_URL) | Mantém, é o banco já em uso no BUILD-eve/Vercel |
| Blob | Disco local (`~/Library/Application Support/minilab-eve/blobs`) |
| Redis | Map em memória local |
| Auth | Vercel (Better Auth) — mantém, não tira |
| Hospedagem dos 3 Eves | **Local**, um por máquina — cada um com seu próprio túnel Cloudflare e URL própria de domínio minilab (ex.: `eve-256.minilab.work`, `work-8gb.minilab.work`, `force-512.minilab.work`) |
| Hospedagem do Places | **ATUALIZADO: Vercel** — porque o iPhone precisa acessar de fora, e Places é um painel único, não uma instalação por máquina |
| Build dos Eves | **ATUALIZADO: local, em cada máquina, disparado por merge no GitHub** (não build centralizado). Isso reaproveita o mecanismo de self-update que já existe (`update.available` via Hub → `verifier.ts`) — é o gancho natural para disparar o build local, em vez de inventar um sistema novo. |
| Personalização do Eve | **NOVO: bandeira colorida por lugar**, abaixo da caixa de prompt, definida no build local a partir do `EVE_PLACE_ID`. Único ponto de diferença visual entre os três Eves. |
| Personalização do Places | **NOVO: detecção de dispositivo em runtime**, não em build — Places é um deploy só, precisa reconhecer de onde o usuário está acessando |
| Hub central | **Continua existindo, não é substituído pelos túneis por Eve.** `wss://places.minilab.work/party/minilab`, rodando no WORK 8GB, porta 1999. Papel: comunicação interna Eve-a-Eve + observabilidade (base do Passo A da seção 6.1). |
| CI/CD | GitHub Actions (build/deploy dos Eves permanece local por máquina; Places vai para Vercel via seu próprio pipeline) |

---

## 4. Os métodos — regras de execução para qualquer agente que pegar este projeto

1. **Ler tudo antes de agir.** `dialogo.md` inteiro e os transcripts relevantes inteiros. Resumo automático do sistema não substitui isso. Não delegar a leitura a subagente sem autorização explícita.
2. **Não perguntar o que já está no contexto.** Se a resposta está lá, usar. Se não está, dizer exatamente o que falta e por quê — nunca perguntar por preguiça de procurar.
3. **Não adivinhar caminho, pasta ou nome de repositório.** Confirmar antes de agir.
4. **Um plano, um arquivo.** Nunca criar um `.md` novo quando já existe um em andamento — editar ou complementar o existente.
5. **Zero improviso durante a execução.** Tudo decidido antes de começar. Cada fase só começa depois de autorização explícita.
6. **Nenhuma ação sem autorização** — `edit`, `exec`, deploy, tunnel, screen sharing — especialmente quando o usuário pediu explicitamente "só responda no chat".
7. **Seguir o padrão documentado do framework em uso** em vez de inventar alternativa por familiaridade (ex.: não trocar filesystem-first por banco de dados sem necessidade real).
8. **Toda fase tem três partes**: (a) reconhecimento de terreno e credenciais antes de começar, (b) execução, (c) verificação pesada ao final, com evidência real — nunca "deveria estar funcionando".
9. **"Completo" é 100%.** Nada de dado simulado sobrando, nada de "quase lá". Se não está testado e comprovado, não está completo.
10. **Handoff limpo.** Ao fim de cada sessão, deixar um resumo escrito em arquivo para o próximo agente — este documento é esse resumo, consolidado.

---

## 5. Anexo técnico cru — arquivos, portas, envs, comandos

*Tudo abaixo é fato extraído literalmente dos transcripts (Devin + Copilot), não inferência. Onde há valor sensível, o valor foi mascarado — o campo em si é real.*

### 5.1 Máquinas e usuário do sistema
| Máquina | Usuário do SO | Roda hoje | Porta |
|---|---|---|---|
| WORK 8GB | `danvoulez` | Hub, Eve 8GB runtime, Places UI | 1999 / 3000 / 4176 |
| BUILD 256 | `ubl-ops` | AGENT-256-eve (parcial), BUILD-eve (a instalar) | 3000 (planejado) |
| FORCE 512 | — | AGENT-512-eve (existe, não ligado ao Hub) | — |

### 5.2 Repositórios GitHub — cuidado com o nome
- `danvoulez/independent-eve` (público, fork) = **BUILD-eve**, o template completo (a peça que falta instalar)
- `danvoulez/agent-256-eve`, `danvoulez/agent-512-eve`, `danvoulez/agent-8gb-eve` (privados) = runtime headless. **Atenção**: localmente as pastas aparecem capitalizadas (`AGENT-256-eve`, `AGENT-8GB-eve`), mas o nome do repo no GitHub é minúsculo. Não confiar no nome da pasta — conferir sempre com `git remote -v`.
- `danvoulez/minilab-8gb-realtime-hub` (privado) = Hub
- `danvoulez/minilab-places` (privado) = Places UI
- Repos que apareceram em pesquisas mas **não fazem parte da decisão atual** (não usar): `danvoulez/eve-chat-template` (é o "mistral-eve", projeto diferente, descartado explicitamente pelo usuário), `danvoulez/dream-machine`, `danvoulez/governed-ui-surface`, `danvoulez/minilab-ui`, `danvoulez/mcp-openai-local-control`.

### 5.3 Portas locais
- Hub: `127.0.0.1:1999` (produção). Uma vez foi testado avulso em `127.0.0.1:2999` — não é a porta real, foi só um teste isolado de protocolo.
- Eve runtime (8GB): `127.0.0.1:3000`
- Places UI: `127.0.0.1:4176` — **atenção**: em planos/sessões mais antigas aparece `4177`. Essa é porta velha, não usar.

### 5.4 URLs públicas (Cloudflare)
- `wss://places.minilab.work/party/minilab` — Hub (rota `/party/*` no túnel de `places.minilab.work`)
- `https://places.minilab.work` — Places UI (catch-all do mesmo túnel)
- `build-256.minilab.work`, `force-512.minilab.work`, `work-8gb.minilab.work`, `eve-256.minilab.work` — hostnames previstos/tentados para os agentes individuais. `work-8gb.minilab.work` e `force-512.minilab.work` já deram **NXDOMAIN** numa sessão — confirmar que o DNS existe antes de assumir.
- Hostnames de configs antigas achados em buscas, tratar como não confiáveis até confirmar: `bringup.minilab.work`, `core.minilab.work`, `library.minilab.work`, `ssh8gb.minilab.work`, `staging-build-256.minilab.work`, `staging-places.minilab.work`.

### 5.5 LaunchAgents (macOS)
**No WORK 8GB, confirmados rodando:**
- `com.minilab.realtime-hub`
- `com.minilab.agent-8gb-eve-runtime`
- `com.minilab.agent-8gb-eve-realtime`
- `com.minilab.places-ui`
- `work.minilab.cloudflared` — já teve bootstrap falhando ("Bootstrap failed: 5: I/O error"). Se repetir: `plutil -lint` no plist, depois `launchctl bootout` + `launchctl bootstrap` por label, um de cada vez — não recarregar todos juntos.

**No BUILD 256, parcial/planejado:**
- `com.minilab.agent-256-eve-runtime`, `com.minilab.agent-256-eve-realtime`, `com.minilab.agent-256-eve-ui`
- `com.minilab.build-eve-ui` (label reservado para quando o BUILD-eve for instalado)
- `com.minilab.build-256.cloudflared`

**Labels de outros projetos no mesmo host** (não confundir com este projeto ao dar `launchctl list | grep minilab`): `com.minilab.ai-factory`, `com.minilab.lab-clock`, `com.minilab.lab-executor`, `com.minilab.lab-radar`, `com.minilab.mcp-factory`, `com.minilab.radar-judge`, `com.minilab.radar-scan`, `com.minilab.actgraph-tunnel`, `com.minilab.actgraph-inference-tunnel`, `com.minilab.host-runtime`, `com.minilab.cloudflared-code247`, `com.minilab.cloudflared-runtime`.

### 5.6 Config do túnel Cloudflare
- Config **ativa** no WORK 8GB: `/etc/cloudflared/config.yml` (nível de sistema, root) — **não** `~/.cloudflared/`. Editar o arquivo errado não tem efeito nenhum e já causou confusão.
- Rota: `hostname: places.minilab.work`, `path: /party/*` → `http://127.0.0.1:1999`
- Catch-all: `hostname: places.minilab.work` → `http://127.0.0.1:4176`
- Reinício: via `sudo -n`, serviço de sistema `com.cloudflare.cloudflared`.

### 5.7 Scripts/runners
- `/Users/danvoulez/minilab-8gb-realtime-hub/run-hub.sh`
- `/Users/danvoulez/AGENT-8GB-eve/run-runtime.sh`
- `/Users/danvoulez/AGENT-8GB-eve/run-realtime.sh`
- `/Users/danvoulez/minilab-places/run-places.sh`
- `npm run start:node` (dentro do Hub) — runner Node real que substitui `partykit dev` em produção (ver armadilha em 5.12).
- `scripts/publish-update.mjs` — publica `update.available` no CI, condicionado a `REALTIME_HUB_URL` estar configurada (sem URL, só gera artifact).
- `agent/skills/self-update/verifier.ts` — recusa update com alvo errado (`target_mismatch`) ou downgrade (commit não descendente do HEAD → `unsafe`).

### 5.8 Variáveis de ambiente reais (valores sensíveis mascarados)
**Eve 8GB** — `/Users/danvoulez/AGENT-8GB-eve/.env`:
```
EVE_ID=eve:agent-8gb
EVE_PLACE_ID=work-8gb
EVE_ROLE=work
REALTIME_HUB_URL=wss://places.minilab.work/party/minilab
REALTIME_HUB_TOKEN=<token>
AI_GATEWAY_API_KEY=<key>
EVE_REALTIME_DISPATCH_MODE=eve.runtime
```

**Places UI** (atual, local — vai mudar com a migração para Vercel da seção 3.3) — `/Users/danvoulez/minilab-places/.env.production`:
```
NEXT_PUBLIC_REALTIME_HUB_URL=wss://places.minilab.work/party/minilab
NEXT_PUBLIC_REALTIME_ROOM=minilab
NEXT_PUBLIC_REALTIME_CLIENT_ID=places:iphone   (varia por instalação: places:lab-256, places:work-8gb)
NEXT_PUBLIC_REALTIME_PLACE_ID=iphone           (varia: build-256, work-8gb)
```

**BUILD-eve** (`.env.example` do template — para quando for instalado):
```
DATABASE_URL=<Neon>
BETTER_AUTH_SECRET=<32+ chars>
BETTER_AUTH_URL=
NEXT_PUBLIC_BETTER_AUTH_URL=
BLOB_READ_WRITE_TOKEN=          → decisão do projeto: trocar por BLOB_LOCAL_PATH=~/Library/Application Support/minilab-eve/blobs
UPSTASH_REDIS_REST_URL=         → decisão do projeto: NÃO usar Upstash, trocar por Map em memória local
UPSTASH_REDIS_REST_TOKEN=       → idem
AI_GATEWAY_API_KEY=<key>
EMBEDDING_PROVIDER=gateway
EMBEDDING_MODEL=openai/text-embedding-3-small   (opcional)
PROJECTION_READER=local
LAB_PROVIDER=local
NEXT_PUBLIC_VERCEL_APP_CLIENT_ID=<Vercel>
VERCEL_APP_CLIENT_SECRET=<Vercel>
```

### 5.9 Secrets do GitHub Actions
Criados (sem expor valor) em 4 repos — `agent-256-eve`, `agent-512-eve`, `agent-8gb-eve`, `minilab-8gb-realtime-hub`:
```
gh secret set REALTIME_HUB_URL
gh secret set REALTIME_HUB_TOKEN
```
Token também salvo localmente em `/Users/ubl-ops/.minilab-secrets/realtime-hub-token` (chmod 600).

### 5.10 Aviso de segurança — isto é fato, não estilo
Nos transcripts apareceram, **em texto puro**:
- Um `MINILAB_GITHUB_WEBHOOK_SECRET` e um `MINILAB_HTTP_SIGNING_SECRET` completos (hex de 64 caracteres cada).
- Um `AI_GATEWAY_VERCEL_TOKEN` completo (formato `vck_...`).
- Referência a um `.env.local` real vazado dentro de um zip antigo de investigação (mistral-eve), junto com `.git/.next/.vercel` de um checkout sujo.

Isso não é opinião: qualquer chave que já apareceu em texto puro num `.md`, transcript ou zip que circulou entre ferramentas (Devin, Copilot, Claude, zips locais) deve ser tratada como potencialmente exposta. **Rotacionar essas chaves é mais barato do que confiar nelas.**

### 5.11 Commits reais já feitos (evidência, não promessa)
- `bf5d040` — "Wire agent chat to realtime hub" (`minilab-places`) — chat da 3ª página ligado ao Hub
- `0c08efc` — "feat: bootstrap agent 8gb eve"
- `0ce7956` — "feat: bootstrap agent 512 eve"
- `352cc34` — "feat: add eve realtime connector"
- `0011470` / `9ddeecb` — "fix: reject self update downgrades"
- `8eff59a` — "Remove malformed pnpm-workspace.yaml and lock with pnpm"

### 5.12 Armadilhas já conhecidas (não repetir, já custou tempo)
- `partykit dev` corrompe o upgrade de WebSocket atrás do Cloudflare — usar sempre o runner Node (`npm run start:node`) em produção, nunca `partykit dev`.
- Next 16 não deixa dois `next dev` do mesmo repo rodarem juntos na mesma porta — matar o processo antigo antes de subir um novo.
- Nome de pasta local ≠ nome do repositório no GitHub (case diferente) — confirmar sempre com `git remote -v`.
- Config ativa do cloudflared no WORK 8GB é `/etc/cloudflared/config.yml` (root), não `~/.cloudflared/`.
- DNS de `work-8gb.minilab.work` e `force-512.minilab.work` já deu NXDOMAIN numa sessão — confirmar antes de apontar qualquer coisa.

---

## 6. O que falta fazer agora (pendência real, em ordem)

1. Instalar o BUILD-eve de verdade em cada máquina (256, 8GB, 512) — não só uma.
2. Dar a cada Eve seu próprio túnel Cloudflare + URL própria de domínio minilab (hoje só o Hub e o Places têm túnel confirmado; os Eves individuais ainda não).
3. Implementar a bandeira colorida por lugar (build-time, a partir de `EVE_PLACE_ID`) e ligar o build local ao merge no GitHub, reaproveitando o mecanismo de self-update já existente.
4. Migrar o Places para o Vercel e implementar a detecção de dispositivo em runtime.
5. **Inventário e reconciliação do Places (substitui o passo genérico "ligar dado real")** — ver metodologia completa na seção 6.1. O Places está completamente unwired: antes de plugar qualquer coisa, inventariar os dois lados e comparar, campo por campo.
6. Conectar os agentes 256 e 512 ao Hub público.
7. Rodar a verificação fim-a-fim completa e produzir o recibo de que cada peça está funcionando de verdade — incluindo os 3 túneis, os 3 Eves com bandeira certa, o Places no Vercel reconhecendo o dispositivo, e a tabela de reconciliação da seção 6.1 fechada (nenhum campo sem fonte, nenhuma fonte sem uso).

### 6.1 Metodologia — Inventário de sinais vitais x Inventário de endpoints do Places

Places hoje mostra dado forjado (`Math.random()`/`setInterval()` em `lib/realtime-client.ts`). O caminho certo não é "trocar o fake por real" direto — é inventariar os dois lados primeiro, comparar, e só então decidir o que liga, o que precisa ser criado, e o que é descartado.

**Passo A — Inventário de sinais vitais disponíveis** (o que já existe no mundo real e deixa rastro, independente do Places):
- **Projeto Manhattan** — infra básica de manutenção do computador. Confirmado como real: aparece como LaunchAgent rodando (`com.project-manhattan.agent`) num dos levantamentos. O que ele expõe (arquivo, socket, log, porta) ainda não foi mapeado nos transcripts — precisa inspeção direta na máquina.
- **Golden Bridge** — infra de bus, segundo você. Não apareceu em nenhum transcript lido até agora — não tenho rastro dele nos arquivos. Precisa ser localizado na máquina (grep por nome, checar LaunchAgents, checar processos) antes de assumir o que ele fornece.
- **Canyon** — túnel exposto. Isto já foi documentado numa sessão anterior: existem "docs Canyon" localmente, e a regra registrada é que **Canyon/Cloudflare é só ingress/transporte, sem autoridade** — ou seja, Canyon não é fonte de estado/dado vital por si só, só carrega o que outra coisa emite.
- **Presença do Eve no Hub** — já funciona hoje de verdade: quando um Eve não responde ao Hub, isso já é sinal de máquina desligada/indisponível. Este é o único sinal dos quatro que já está, comprovadamente, ligado a algo real.

Este é o ponto de partida, não é a lista fechada. Mapear de verdade exige acesso às 3 máquinas (SSH, `launchctl list`, `find`/`rg` por Manhattan/Golden Bridge/Canyon, ver onde cada um grava estado) — isso não pode ser feito a partir desta conversa, precisa ser a primeira tarefa de quem tiver acesso ao sistema.

**Passo B — Inventário de endpoints/slots do Places** (o que a UI hoje espera preencher):
- `lib/realtime-client.ts` — hoje forja três campos: `placeStatus`, `jobProgress`, `nodeHeartbeat`. Esses três já são "slots" reais da UI, só que alimentados por dado falso — são o primeiro alvo natural de reconciliação.
- `lib/place-catalog.ts` — mapeia os places e provavelmente seus metadados (nome, id, Eve associado).
- `AgentChat.tsx` — já está ligado de verdade (`chat.message`/`chat.response` via Hub) — não é slot pendente, é referência do que "ligado direito" parece.
- Console realtime que existia em `app/page.tsx` (home) foi removido numa sessão anterior — checar se ele consumia algum outro slot que ficou órfão depois da remoção.
- Levantar a lista completa exige ler o código-fonte atual de `minilab-places` campo por campo — também é tarefa de quem tiver o repo em mãos, não algo que dá pra fechar só com os transcripts.

**Passo C — Reconciliar A x B, produzir uma tabela única:**

| Campo do Places | Fonte real disponível? | Ação |
|---|---|---|
| `placeStatus` | Presença do Eve no Hub cobre parte disso | Ligar direto ao Hub; checar se falta granularidade (ex.: "ligado mas ocupado") |
| `nodeHeartbeat` | Depende do que Manhattan/Golden Bridge expõem — não mapeado ainda | Mapear Manhattan/Golden Bridge antes de decidir |
| `jobProgress` | Nenhuma fonte identificada até agora | Decidir: instrumentar o Eve para emitir isso, ou remover o campo da UI se não fizer mais sentido |

(Esta tabela é o esqueleto — vazia de propósito nas partes que dependem de acesso à máquina. Preencher linha por linha é o trabalho da próxima sessão com acesso real ao sistema.)

Regra de fechamento: **nenhum campo do Places fica sem fonte identificada, e nenhuma fonte vital mapeada fica sem decisão sobre usar ou não.** Só depois disso o critério de "Places 100% completo" (definido no plano original, `PLANO-PLACES-8GB.md`) deixa de ser aspiração e vira fato verificável.

