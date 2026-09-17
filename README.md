<div align="center">

<img src="assets/clawfast.svg" alt="CLAWFAST — com o clawfast você faz tudo" width="760">

# CLAWFAST

### Com o clawfast você faz tudo.

**Um agente de pentest autônomo que vive no seu terminal.** Você fala em português, ele pensa, escreve, executa e prova. Recon, exploração, auditoria de código, relatórios — tudo na linha de comando, com **as suas chaves**, **sem login** e **sem limite de uso**.

</div>

---

## Índice

- [O que é o clawfast — e por que existe](#o-que-é-o-clawfast--e-por-que-existe)
- [Instalação — sem npm, sem Node](#instalação--sem-npm-sem-node)
- [Por que abandonamos o npm](#por-que-abandonamos-o-npm)
- [Primeiro contato](#primeiro-contato)
- [Como usar](#como-usar)
- [Controles enquanto ele trabalha — o start/stop](#controles-enquanto-ele-trabalha--o-startstop)
- [Atualização — baixa uma vez, clawfast upgrade cuida do resto](#atualização--baixa-uma-vez-clawfast-upgrade-cuida-do-resto)
- [Comandos de barra](#comandos-de-barra)
- [Auditoria de projeto — modo só-leitura](#auditoria-de-projeto--modo-só-leitura)
- [Rede, alvos e o escopo sagrado](#rede-alvos-e-o-escopo-sagrado)
- [Avaliação multi-agente](#avaliação-multi-agente)
- [Garimpo com Google Dorks](#garimpo-com-google-dorks)
- [Cérebro externo: /conect](#cérebro-externo-conect)
- [Ensine novos truques: Skills](#ensine-novos-truques-skills)
- [Modelos e fallback automático](#modelos-e-fallback-automático)
- [Sobre o código e a segurança](#sobre-o-código-e-a-segurança)
- [Referência rápida](#referência-rápida)

---

## O que é o clawfast — e por que existe

Imagine abrir o terminal, digitar um pedido em linguagem natural e ver um operador de segurança trabalhar de verdade na sua frente: rodando comandos, fazendo requisições HTTP, mapeando alvos, encontrando vulnerabilidades e escrevendo o laudo. Não é autocomplete. É um **agente autônomo** com ferramentas reais nas mãos.

**Por que ele existe:** as ferramentas de pentest vivem espalhadas — um scanner aqui, um script ali, o relatório no fim, tudo desconectado. O clawfast junta tudo num só lugar, guiado por linguagem natural, e roda **na sua máquina**, com **as suas próprias chaves de modelo**, **sem login** e **sem limite de uso**. O que você manda fazer, ele faz.

> Você comanda. Ele executa. O terminal vira sua arma.

---

## Instalação — sem npm, sem Node

**Um comando. Sem instalar Node, sem npm, sem baixar código-fonte.** O clawfast é distribuído como um **executável único** — o runtime já vem dentro dele.

**Windows (PowerShell):**

```powershell
& ([scriptblock]::Create((Invoke-WebRequest -UseBasicParsing 'https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.ps1').Content))
```

**Linux / macOS:**

```sh
curl -fsSL https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.sh | sh
export PATH="$HOME/.clawfast-runtime/bin:$PATH"
```

Plataformas: **Windows x64**, **Linux x64**, **macOS Intel** e **macOS Apple Silicon**.

**O que o instalador faz — e por quê:**

- Baixa o executável da release oficial no GitHub e **confere o SHA-256** antes de instalar. *Por quê:* garante que o binário não foi corrompido ou trocado no caminho.
- Instala em `~/.clawfast-runtime/` e cria o comando `clawfast`. *Por quê:* fica isolado numa pasta sua, sem sujar o sistema, e cada versão fica guardada para rollback.
- **Não precisa de Node nem npm.** *Por quê:* o runtime de JavaScript já está embutido no executável — a pessoa não instala nada além do próprio clawfast.

No Linux/macOS, adicione a linha `export PATH` ao arquivo de inicialização do seu shell (`~/.bashrc`, `~/.zshrc`) para o comando persistir em novos terminais. Depois, confira:

```sh
clawfast version
```

---

## Por que abandonamos o npm

Antes, o clawfast era instalado por `npm install -g clawfast`. **Paramos com isso — e aqui está o motivo, sem rodeios:**

A verificação automática de segurança do registro **npm começou a bloquear as publicações** do clawfast (as versões 2.11.1, 2.11.2 e 2.11.3 foram barradas). Enquanto isso, a tag `latest` no npm continuava presa numa versão antiga — então quem rodava `update` **continuava recebendo a versão velha**, sem as novidades.

A solução foi cortar a dependência do npm de vez: agora o clawfast é entregue como **executável verificado por SHA-256 direto do GitHub Releases**. Instala uma vez pelo comando acima e, a partir daí, `clawfast upgrade` baixa as próximas versões do GitHub — **sem npm no caminho, sem nada travando a distribuição**.

Foi exatamente isso que levou o projeto ao ciclo **3.x**: a numeração marca a virada de canal de distribuição, não uma reescrita nem uma quebra de compatibilidade.

---

## Primeiro contato

Abra o terminal e digite:

```sh
clawfast
```

Na primeira vez, ele pede **uma chave de modelo** — só isso. Use a [NVIDIA build](https://build.nvidia.com/), que dá acesso a **modelos de ponta gratuitos**:

1. Crie a chave em **https://build.nvidia.com/**
2. Cole quando o clawfast pedir.

A chave fica salva em `~/.clawfast/.env`. **Por que guardar localmente:** é a sua chave, na sua máquina — sem servidor no meio, sem login, sem ninguém vendo o que você faz.

---

## Como usar

Depois que o banner sobe, **você simplesmente conversa**. Escreva o que quer, em português, e deixe o agente trabalhar:

```text
❯ faça um recon completo de example.com
❯ teste essa API por IDOR e me mostre a prova
❯ crie um script python que valida esse XSS e rode
❯ analise o meu projeto e me diga onde estão as falhas
❯ gere um relatório do que encontramos até agora
```

Ele decide as ferramentas, executa comando por comando, mostra a saída em tempo real e entrega o resultado. **Por que isso muda o jogo:** você não decora a sintaxe de dez ferramentas diferentes — descreve a intenção e acompanha a execução, podendo interromper, redirecionar ou aprofundar a qualquer momento.

---

## Controles enquanto ele trabalha — o start/stop

Ele pode estar no meio de qualquer coisa: um scan, um exploit, dez processos abertos. **Você manda parar e para tudo, na hora.**

| Tecla | O que faz |
|-------|-----------|
| `F4` | **INICIAR / PARAR** — o botão que vive no topo da caixa de input. PARAR mata tudo de uma vez: o turno do modelo, a fila de mensagens, as ferramentas rodando, as sessões vivas (msfconsole, sqlmap, ssh, banco) e a **árvore inteira de processos** |
| `F2` e clicar | libera o mouse e clica no botão `[ INICIAR ]` / `[ PARAR ]` direto na tela |
| `Ctrl+C` (1x) | interrompe a tarefa atual |
| `Ctrl+C` (2x em 2s) | fecha o clawfast |
| `↑` / `↓` | navega pelo histórico de comandos |
| digitar durante a execução | manda texto direto para o programa em andamento (REPLs, prompts, `sudo`…) |

**Por que o start/stop existe:** um agente autônomo com ferramentas reais pode disparar um comando pesado, um scan longo ou um subprocesso que abre outros processos. Você precisa de **um freio de emergência de verdade** — não um "cancelar" que deixa coisa rodando por baixo. O PARAR aborta o modelo, esvazia a fila, encerra as sessões e derruba a árvore inteira de processos (no Windows via `taskkill /T`), então nada fica órfão consumindo a sua máquina ou tocando o alvo.

---

## Atualização — baixa uma vez, clawfast upgrade cuida do resto

Depois de instalado, atualizar é um comando:

```sh
clawfast upgrade
```

(`clawfast update` faz o mesmo.) O que acontece — e por que é seguro:

1. Ele consulta a **última release** no GitHub. Se você já está na mais nova, avisa e não faz nada.
2. Se houver versão nova, baixa o **novo executável**, **confere o SHA-256** e **troca o binário** em `~/.clawfast-runtime/`, guardando o anterior numa pasta por versão. *Por quê:* se algo falhar no download ou na verificação, o executável antigo continua funcionando — você nunca fica sem o clawfast.
3. **Nunca baixa código-fonte** — só o executável, o instalador e os checksums. O código continua fechado.

E tem um aviso automático: no boot, o clawfast checa o GitHub no máximo **1x por dia** (em segundo plano) e, na próxima vez que você abrir, mostra sozinho *"nova versão disponível — rode clawfast update"*. Depois do upgrade, **feche e abra o clawfast** para usar a versão nova.

```sh
clawfast --version   # mostra a versão instalada
```

---

## Comandos de barra

Digite `/` a qualquer momento para abrir o menu ao vivo:

| Comando | O que faz |
|---------|-----------|
| `/model` | troca o modelo da sessão num seletor com setas |
| `/api` | troca sua chave NVIDIA na hora — testa, valida e ativa sem reiniciar |
| `/skills` | lista as skills instaladas |
| `/skillcreator` | cria uma nova skill (ensina o clawfast a fazer algo do seu jeito) |
| `/conect` | conecta uma IA externa (Claude Code, Codex, Gemini…) para dirigir o clawfast via MCP |
| `/system` | salva o cérebro do agente (system prompt) num visualizador HTML |
| `/nov` | mostra as novidades desta versão |
| `/exit` | fecha o clawfast |

---

## Auditoria de projeto — modo só-leitura

Entre na pasta do seu projeto e rode `clawfast` ali mesmo. Peça uma auditoria e ele **vira um auditor dedicado, somente-leitura** — sozinho, sem comando especial:

```text
❯ analise o meu projeto e procure vulnerabilidades e segredos
```

A partir daí, ele:

- **Mapeia o projeto inteiro** — grafo de imports nos dois sentidos, achando imports quebrados, ciclos e arquivos órfãos.
- **Caça vulnerabilidades de verdade** — SQLi, injeção de comando/código, XSS, TLS desligado, cripto fraca, CORS aberto, chaves e tokens embutidos (mascarados no laudo).
- **Orquestra ferramentas de ponta** — combina análise determinística com o raciocínio do modelo para rastrear o fluxo de dados de ponta a ponta.
- **Só aceita o que pode provar** — descarta achado sem caminho confirmado no código real.

**Por que o modo é só-leitura:** auditar não é editar. Nesse modo ele **nunca cria, altera ou apaga nada** no seu projeto — a única coisa que ele escreve é **um relatório** `ANALISE_PROJETO_<data>.md` na raiz. Seu código fica intocado, e você confia no laudo sabendo que ele não mexeu em nada. Quer voltar ao modo de ataque? Diga: **"modo normal"**.

---

## Rede, alvos e o escopo sagrado

O clawfast não é teórico — ele **fala com a rede**:

- **Requisições HTTP nativas** — GET, POST, headers, cookies, payloads. Ele bate no alvo e lê a resposta.
- **Recon de superfície completo** — descoberta de subdomínios, varredura de portas, coleta de conteúdo e inteligência sobre o alvo.
- **Execução real** — roda os scanners e ferramentas que você tem instalados (e te ajuda a instalar os que faltam).
- **Verificação de exploit** — não basta "parece vulnerável": ele monta a prova e executa para confirmar.

**Por que o escopo é sagrado:** poder de execução real exige uma fronteira real. Você define o alvo; se qualquer parte do agente for tocar em algo **fora do escopo** — um formulário que envia dados para outro host, um probe num alvo não autorizado — a operação **para na hora** e te mostra exatamente o quê e onde, **sem tocar**. Você autoriza (ou não) e manda seguir. O clawfast **nunca sai do escopo sozinho**.

> Use o clawfast apenas em sistemas que você tem **autorização explícita** para testar.

---

## Avaliação multi-agente

Peça uma avaliação completa e o clawfast deixa de rodar uma ferramenta por vez — um **Orquestrador** sobe **quatro agentes especialistas que trabalham juntos** no mesmo alvo, conversando entre si e compartilhando um mapa vivo do sistema:

```text
❯ faça uma avaliação completa de https://example.com
❯ pentesta meu sistema: o site é https://app.exemplo.com e o código está em ./
```

| Agente | O que faz |
|--------|-----------|
| 🔎 **Discovery** | mapeia a superfície — links, formulários, tecnologias, endpoints |
| 🌐 **Web** | testa o comportamento em runtime, de forma segura — status, headers, CORS, reflexão |
| 🧬 **Source** | audita o seu código (somente leitura) — acha rotas e fluxos de dados perigosos |
| 🧪 **Validator** | confirma cada achado com prova — e é honesto quando não dá para provar |

**Por que quatro agentes em vez de um scanner:** o pulo do gato é a **correlação**. Quando o Source acha um parâmetro do código que cai num ponto perigoso (um `fetch`, um `exec`, uma query) **e** esse mesmo parâmetro aparece exposto num endpoint que o Discovery mapeou, o clawfast junta os dois num achado de **alta confiança** — o tipo de falha (SSRF, injeção) que scanners isolados não enxergam porque olham código e superfície separados.

Tudo que os agentes descobrem alimenta um **grafo do alvo** persistente, e o clawfast te diz o **próximo passo** com base nele. Você acompanha ao vivo:

```text
╭──────────────────────────────────────────────╮
│ CLAWFAST MULTI-AGENT                          │
│ Target: example.com                           │
├──────────────────────────────────────────────┤
│ [*] Discovery      RUNNING                    │
│ [*] Web Analysis   RUNNING                    │
│ [*] Source Audit   RUNNING                    │
│ [ ] Verification   WAITING                    │
├──────────────────────────────────────────────┤
│ Assets        14                              │
│ Endpoints     83                              │
│ Parameters    219                             │
│ Findings      7                               │
│ Confirmed     2                               │
╰──────────────────────────────────────────────╯
```

---

## Garimpo com Google Dorks

Recon por operadores de busca — **de graça e sem chave**, sobre DuckDuckGo **e** Bing. Você diz a intenção e o clawfast monta a sintaxe; ou dispara um **pacote pronto** contra um domínio:

```text
❯ roda os dorks de segredos e backups em example.com
❯ procura painéis de admin e diretórios abertos em example.com
```

São pacotes no estilo **Google Hacking Database** para segredos e chaves, backups, arquivos de config, `.git` exposto, painéis de admin, listagem de diretórios e dados vazados no GitHub/Pastebin/S3. **Por que sem chave:** recon inicial não deveria custar nem depender de API paga — ele usa os buscadores públicos e ainda abre os melhores resultados para você já ler.

---

## Cérebro externo: /conect

Quer um modelo de fronteira dirigindo o clawfast? Digite `/conect` e aponte o **Claude Code, Codex, Gemini ou Qwen** para ele. A IA externa vira o cérebro; o clawfast continua sendo as **mãos e os olhos** — ela chama as ferramentas, você vê tudo acontecer no seu terminal.

**Por que isso é diferente de "uma IA conversando com outra":** aqui a IA conectada **usa o clawfast como ferramenta**, com todo o arsenal na mão — inclusive a avaliação multi-agente. Tudo preso em `127.0.0.1` e protegido por token, então nada disso sai da sua máquina. Configure uma vez, esqueça.

---

## Ensine novos truques: Skills

Com `/skillcreator` você cola ou escreve uma **skill** — uma técnica, um checklist, um playbook do seu jeito — de qualquer tamanho:

```text
❯ /skillcreator
nome da skill?      ❯ xss-recon
descrição curta?    ❯ rotina de caça a XSS refletido e armazenado
palavras-gatilho?   ❯ xss, cross-site scripting
cole a skill agora  ❯ (cole o conteúdo... termine com /fim)
```

**Por que skills e não só um prompt gigante:** a skill vira conhecimento disponível para **todos os modelos**, mas o conteúdo completo só é carregado quando o seu pedido casa com ela. *Por quê:* isso mantém o contexto leve e barato — o playbook pesado só entra quando é útil, em vez de ocupar espaço em toda conversa. Liste com `/skills`, remova com `/skill delete <nome>`.

---

## Modelos e fallback automático

O clawfast tem uma **cadeia de fallback automática** — se um modelo falha ou bate em limite, ele passa para o próximo sozinho. **Por quê:** modelos gratuitos batem em rate limit (429) o tempo todo; sem fallback, você travaria no meio de um pentest. Com ele, a sessão continua sem você perceber.

Provedor primário: **NVIDIA build** (gratuito), com modelos de função de verdade — as ferramentas disparam de fato:

- `mistralai/mistral-medium-3.5`
- `openai/gpt-oss-120b`
- `z-ai/glm-5.1`
- `qwen/qwen3.5-397b`

Quer fixar um modelo? Abra `/model` e escolha com as setas. Quer voltar ao automático? `/model auto`.

---

## Sobre o código e a segurança

Sendo transparente sobre o que o executável é:

- O binário **não contém o código-fonte** do projeto — ele traz o JavaScript **empacotado e minificado** com o runtime embutido. Você não recebe os arquivos-fonte, o repositório nem o build.
- Um executável compilado **ainda pode ser analisado** por alguém habilidoso (dá para extrair o bundle minificado). Contra usuário comum: fechado. Contra engenharia reversa determinada: não é segredo absoluto — os checksums protegem contra corrupção, não substituem a confiança na conta do GitHub.
- As ferramentas externas que o clawfast usa durante uma sessão podem exigir dependências próprias; **só o runtime do CLI** está incluído no executável.

E o de sempre, que vale repetir: **use o clawfast apenas em sistemas que você tem autorização explícita para testar.**

---

## Referência rápida

**Instalar (Windows / PowerShell):**

```powershell
& ([scriptblock]::Create((Invoke-WebRequest -UseBasicParsing 'https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.ps1').Content))
```

**Instalar (Linux / macOS):**

```sh
curl -fsSL https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.sh | sh
export PATH="$HOME/.clawfast-runtime/bin:$PATH"
```

**No dia a dia:**

```sh
clawfast              # abre o agente
clawfast upgrade      # atualiza para a última versão (pelo GitHub, sem npm)
clawfast --version    # mostra a versão instalada
```

**Dentro do agente:**

```text
/model   /api   /skills   /skillcreator   /conect   /system   /nov   /exit
```

---

<div align="center">

### O terminal sempre foi seu. Agora ele trabalha por você.

**Com o clawfast você faz tudo.**

</div>
