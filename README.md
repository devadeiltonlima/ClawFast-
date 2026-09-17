<div align="center">

<img src="assets/clawfast.svg" alt="CLAWFAST, com o clawfast você faz tudo" width="760">

# CLAWFAST

### Com o clawfast você faz tudo.

Um agente de pentest que mora no seu terminal. Você fala em português, ele pensa, escreve, executa e prova. Recon, exploração, auditoria de código, relatório, tudo na linha de comando, com as suas chaves, sem login e sem limite de uso.

</div>

---

## O que é o clawfast (e por que ele existe)

Abre o terminal, digita um pedido em português e vê um operador de segurança trabalhando de verdade na sua frente. Ele roda comando, faz requisição HTTP, mapeia alvo, acha vulnerabilidade e escreve o laudo. Não é autocomplete, é um agente com ferramentas reais na mão.

Fiz o clawfast porque as ferramentas de pentest vivem espalhadas. Um scanner aqui, um script ali, o relatório lá no fim, cada coisa num canto. A ideia foi juntar tudo num lugar só, guiado por conversa, rodando na sua máquina, com as suas próprias chaves de modelo, sem login e sem ninguém contando quantas vezes você usou.

> Você comanda, ele executa. O terminal vira sua arma.

---

## Instalação (sem npm, sem Node)

Um comando e acabou. Não instala Node, não usa npm, não baixa código-fonte. O clawfast vem como um executável único, com o runtime já embutido dentro dele.

**Windows (PowerShell):**

```powershell
$s = Join-Path $env:TEMP 'clawfast-install.ps1'; Invoke-WebRequest -UseBasicParsing 'https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.ps1' -OutFile $s; & ([scriptblock]::Create((Get-Content -Raw $s)))
```

**Linux e macOS:**

```sh
curl -fsSL https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.sh | sh
export PATH="$HOME/.clawfast-runtime/bin:$PATH"
```

Tem para Windows x64, Linux x64, macOS Intel e macOS Apple Silicon.

O instalador baixa o executável da release oficial no GitHub e confere o SHA-256 antes de instalar, então você sabe que o binário chegou inteiro e não foi trocado no caminho. Ele coloca tudo em `~/.clawfast-runtime/` e cria o comando `clawfast`, num canto isolado que não suja o resto do sistema e ainda guarda cada versão para rollback. E o principal: nada de Node nem npm, porque o runtime de JavaScript já mora dentro do executável.

No Linux e no macOS, joga a linha do `export PATH` no seu `~/.bashrc` ou `~/.zshrc` para o comando continuar existindo nos próximos terminais. Depois confere:

```sh
clawfast version
```

---

## Por que larguei o npm

Antes o clawfast era `npm install -g clawfast`. Parei com isso, e o motivo é bem direto.

A verificação automática de segurança do npm começou a barrar as publicações do clawfast. As versões 2.11.1, 2.11.2 e 2.11.3 foram bloqueadas, e enquanto isso a tag `latest` no npm ficava travada numa versão velha. Resultado: quem rodava `update` continuava baixando a versão antiga, sem novidade nenhuma.

A saída foi cortar o npm de vez. Agora o clawfast sai como executável verificado por SHA-256 direto do GitHub Releases. Instala uma vez pelo comando lá de cima e, daí em diante, `clawfast upgrade` puxa as próximas versões do GitHub, sem npm no meio, sem nada travando a entrega. Foi isso que empurrou o projeto pro ciclo 3.x. O número mudou porque o canal de distribuição mudou, não porque teve reescrita ou quebra de compatibilidade.

---

## Primeiro contato

Abre o terminal e digita:

```sh
clawfast
```

Na primeira vez ele pede uma chave de modelo, só isso. Recomendo a [NVIDIA build](https://build.nvidia.com/), que dá modelos de ponta de graça:

1. Cria a chave em **https://build.nvidia.com/**
2. Cola quando o clawfast pedir.

A chave fica salva em `~/.clawfast/.env`, na sua máquina. Sem servidor no meio, sem login, sem ninguém do outro lado vendo o que você faz. É a sua chave e o seu terminal.

---

## Como usar

Depois que o banner sobe, você só conversa. Escreve o que quer, em português, e deixa ele trabalhar:

```text
❯ faça um recon completo de example.com
❯ teste essa API por IDOR e me mostre a prova
❯ crie um script python que valida esse XSS e rode
❯ analise o meu projeto e me diga onde estão as falhas
❯ gere um relatório do que encontramos até agora
```

Ele escolhe as ferramentas, executa comando por comando, mostra a saída ao vivo e entrega o resultado. A graça é essa: você não precisa decorar a sintaxe de dez ferramentas diferentes, só descreve a intenção e acompanha na tela, podendo interromper, redirecionar ou aprofundar na hora que quiser.

---

## O start/stop: parar tudo na hora

Ele pode estar no meio de qualquer coisa. Um scan, um exploit, dez processos abertos. Você manda parar e para tudo, na hora.

| Tecla | O que faz |
|-------|-----------|
| `F4` | INICIAR e PARAR, o botão que fica no topo da caixa de input. O PARAR mata tudo de uma vez: o turno do modelo, a fila de mensagens, as ferramentas rodando, as sessões vivas (msfconsole, sqlmap, ssh, banco) e a árvore inteira de processos |
| `F2` e clicar | libera o mouse e clica no botão `[ INICIAR ]` ou `[ PARAR ]` direto na tela |
| `Ctrl+C` (1x) | interrompe a tarefa atual |
| `Ctrl+C` (2x em 2s) | fecha o clawfast |
| `↑` e `↓` | navega pelo histórico de comandos |
| digitar durante a execução | manda texto direto pro programa em andamento (REPLs, prompts, `sudo`) |

Botei o start/stop porque um agente autônomo com ferramentas reais na mão pode disparar um comando pesado, um scan longo ou um processo que abre outros processos. Você precisa de um freio de emergência que freia de verdade, não de um "cancelar" bonitinho que deixa coisa rodando por baixo dos panos. O PARAR aborta o modelo, esvazia a fila, encerra as sessões e derruba a árvore inteira de processos (no Windows via `taskkill /T`), então nada fica órfão comendo a sua máquina ou cutucando o alvo.

---

## Atualizar é um comando só

Depois de instalado, atualizar é isso:

```sh
clawfast upgrade
```

O `clawfast update` faz a mesma coisa. Por dentro ele age assim, e com cuidado: primeiro consulta a última release no GitHub, e se você já está na mais nova, avisa e não faz nada. Se tiver versão nova, ele baixa o executável novo, confere o SHA-256 e troca o binário em `~/.clawfast-runtime/`, guardando o anterior numa pasta por versão. Se algo der errado no download ou na conferência, o executável antigo continua no lugar, então você nunca fica sem o clawfast. E ele nunca baixa código-fonte, só o executável, o instalador e os checksums. O código continua fechado.

Ainda tem o aviso automático. No boot ele checa o GitHub no máximo uma vez por dia, em segundo plano, e na próxima vez que você abrir aparece sozinho um "nova versão disponível, rode clawfast update". Depois de atualizar, fecha e abre o clawfast pra rodar a versão nova.

```sh
clawfast --version
```

---

## Comandos de barra

Digita `/` a qualquer momento pra abrir o menu ao vivo:

| Comando | O que faz |
|---------|-----------|
| `/model` | troca o modelo da sessão num seletor com setas |
| `/api` | troca sua chave NVIDIA na hora, testa, valida e ativa sem reiniciar |
| `/skills` | lista as skills instaladas |
| `/skillcreator` | cria uma skill nova (ensina o clawfast a fazer algo do seu jeito) |
| `/conect` | conecta uma IA externa (Claude Code, Codex, Gemini) pra dirigir o clawfast via MCP |
| `/system` | salva o cérebro do agente (system prompt) num visualizador HTML |
| `/nov` | mostra as novidades da versão |
| `/exit` | fecha o clawfast |

---

## Auditar um projeto (modo só leitura)

Entra na pasta do seu projeto e roda `clawfast` ali mesmo. Pede uma auditoria e ele vira um auditor dedicado, só leitura, sozinho, sem precisar de comando especial:

```text
❯ analise o meu projeto e procure vulnerabilidades e segredos
```

A partir daí ele mapeia o projeto inteiro, montando o grafo de imports nos dois sentidos pra achar import quebrado, ciclo e arquivo órfão. Vai atrás de vulnerabilidade de verdade: SQLi, injeção de comando e de código, XSS, TLS desligado, cripto fraca, CORS aberto, chave e token embutidos (que aparecem mascarados no laudo). Ele combina análise determinística com o raciocínio do modelo pra rastrear o fluxo de dados de ponta a ponta, e só aceita o que consegue provar, jogando fora todo achado sem caminho confirmado no código real.

Fiz o modo assim porque auditar não é editar. Aqui ele nunca cria, altera ou apaga nada no seu projeto. A única coisa que ele escreve é um relatório `ANALISE_PROJETO_<data>.md` na raiz. Seu código fica intocado, e você confia no laudo sabendo que ele não pôs a mão em nada. Quer voltar pro modo de ataque? Só falar "modo normal".

---

## Rede, alvos e o escopo

O clawfast não é teórico, ele fala com a rede. Faz requisição HTTP nativa (GET, POST, headers, cookies, payloads, ele bate no alvo e lê a resposta), roda recon de superfície completo (descoberta de subdomínio, varredura de porta, coleta de conteúdo e inteligência sobre o alvo), executa de verdade os scanners e ferramentas que você tem instalados (e te ajuda a instalar os que faltam) e faz verificação de exploit, porque "parece vulnerável" não basta, ele monta a prova e roda pra confirmar.

E o escopo é sagrado. Poder de execução real pede uma fronteira real. Você define o alvo, e se qualquer parte do agente for tocar em algo fora do escopo, um formulário que manda dado pra outro host, um probe num alvo não autorizado, a operação para na hora e te mostra exatamente o quê e onde, sem tocar. Você autoriza (ou não) e manda seguir. Ele nunca sai do escopo sozinho.

> Use o clawfast só em sistemas que você tem autorização explícita pra testar.

---

## Avaliação multi-agente

Pede uma avaliação completa e o clawfast para de rodar uma ferramenta por vez. Um Orquestrador sobe quatro agentes especialistas que trabalham juntos no mesmo alvo, conversando entre si e dividindo um mapa vivo do sistema:

```text
❯ faça uma avaliação completa de https://example.com
❯ pentesta meu sistema: o site é https://app.exemplo.com e o código está em ./
```

| Agente | O que faz |
|--------|-----------|
| 🔎 **Discovery** | mapeia a superfície: links, formulários, tecnologias, endpoints |
| 🌐 **Web** | testa o comportamento em runtime, de forma segura: status, headers, CORS, reflexão |
| 🧬 **Source** | audita o seu código (só leitura), achando rotas e fluxos de dados perigosos |
| 🧪 **Validator** | confirma cada achado com prova, e é honesto quando não dá pra provar |

O pulo do gato não é ter quatro agentes, é a correlação. Quando o Source acha um parâmetro do código que cai num ponto perigoso (um `fetch`, um `exec`, uma query) e esse mesmo parâmetro aparece exposto num endpoint que o Discovery mapeou, o clawfast junta os dois num achado de alta confiança. É o tipo de falha, tipo SSRF ou injeção, que scanner isolado não enxerga, porque olha código e superfície separados.

Tudo que os agentes descobrem vira nó num grafo do alvo que persiste, e o clawfast te diz o próximo passo com base nele. E você acompanha ao vivo:

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

## Google Dorks

Recon por operador de busca, de graça e sem chave, em cima do DuckDuckGo e do Bing. Você diz a intenção e o clawfast monta a sintaxe, ou dispara um pacote pronto contra um domínio:

```text
❯ roda os dorks de segredos e backups em example.com
❯ procura painéis de admin e diretórios abertos em example.com
```

São pacotes no estilo Google Hacking Database pra segredo e chave, backup, arquivo de config, `.git` exposto, painel de admin, listagem de diretório e dado vazado no GitHub, Pastebin ou S3. É sem chave de propósito, porque recon inicial não devia custar nem depender de API paga. Ele usa os buscadores públicos e ainda abre os melhores resultados pra você já ir lendo.

---

## /conect: um cérebro externo dirigindo

Quer um modelo de fronteira dirigindo o clawfast? Digita `/conect` e aponta o Claude Code, Codex, Gemini ou Qwen pra ele. A IA externa vira o cérebro e o clawfast continua sendo as mãos e os olhos. Ela chama as ferramentas, você vê tudo acontecer no seu terminal.

Não é uma IA conversando com a outra. Aqui a IA conectada usa o clawfast como ferramenta, com todo o arsenal na mão, inclusive a avaliação multi-agente. E fica tudo preso em `127.0.0.1`, protegido por token, então nada disso sai da sua máquina. Configura uma vez e esquece.

---

## Skills: ensinando truques novos

Com `/skillcreator` você cola ou escreve uma skill, que é uma técnica, um checklist, um playbook do seu jeito, de qualquer tamanho:

```text
❯ /skillcreator
nome da skill?      ❯ xss-recon
descrição curta?    ❯ rotina de caça a XSS refletido e armazenado
palavras-gatilho?   ❯ xss, cross-site scripting
cole a skill agora  ❯ (cole o conteúdo... termine com /fim)
```

A skill vira conhecimento disponível pra todos os modelos, mas o conteúdo completo só carrega quando o seu pedido casa com ela. Fiz assim pra manter o contexto leve e barato: o playbook pesado só entra quando é útil, em vez de ficar ocupando espaço em toda conversa. Lista com `/skills` e remove com `/skill delete <nome>`.

---

## Modelos e o fallback

O clawfast tem uma cadeia de fallback automática. Se um modelo falha ou bate no limite, ele passa pro próximo sozinho. Isso importa porque modelo grátis bate em rate limit (429) o tempo todo, e sem fallback você travaria no meio de um pentest. Com ele, a sessão segue e você nem percebe.

O provedor primário é a NVIDIA build (gratuita), com modelos de função de verdade, aqueles em que as ferramentas disparam mesmo:

- `mistralai/mistral-medium-3.5`
- `openai/gpt-oss-120b`
- `z-ai/glm-5.1`
- `qwen/qwen3.5-397b`

Quer fixar um modelo? Abre o `/model` e escolhe com as setas. Quer voltar pro automático? `/model auto`.

---

## Sobre o código (e a real sobre segurança)

Sendo honesto com você sobre o que é o executável:

O binário não tem o código-fonte do projeto dentro. Ele carrega o JavaScript empacotado e minificado junto com o runtime. Você não recebe os arquivos-fonte, nem o repositório, nem o build.

Mas, pra não te vender ilusão: um executável compilado ainda pode ser analisado por alguém habilidoso, dá pra extrair o bundle minificado de dentro dele. Contra usuário comum, é fechado. Contra engenharia reversa determinada, não é segredo absoluto. Os checksums protegem contra corrupção, não substituem a confiança na conta do GitHub.

E as ferramentas externas que o clawfast usa durante uma sessão podem pedir dependências próprias. Só o runtime do CLI está incluído no executável.

O de sempre, que vale repetir: use o clawfast só em sistemas que você tem autorização explícita pra testar.

---

## Referência rápida

**Instalar no Windows (PowerShell):**

```powershell
$s = Join-Path $env:TEMP 'clawfast-install.ps1'; Invoke-WebRequest -UseBasicParsing 'https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.ps1' -OutFile $s; & ([scriptblock]::Create((Get-Content -Raw $s)))
```

**Instalar no Linux ou macOS:**

```sh
curl -fsSL https://github.com/devadeiltonlima/ClawFast-/releases/latest/download/install.sh | sh
export PATH="$HOME/.clawfast-runtime/bin:$PATH"
```

**No dia a dia:**

```sh
clawfast              # abre o agente
clawfast upgrade      # atualiza pela última versão (pelo GitHub, sem npm)
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
