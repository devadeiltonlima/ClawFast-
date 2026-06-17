<div align="center">

```
 █████  ██      ████  ██   ██ ██████  ████   ██████ ██████
██      ██     ██  ██ ██   ██ ██     ██  ██  ██       ██
██      ██     ██████ ██ █ ██ █████  ██████  ██████   ██
██      ██     ██  ██ ██ █ ██ ██         ██      ██   ██
 █████  ██████ ██  ██ ███████ ██     █████  ██████    ██
```

# CLAWFAST

### Com o clawfast você faz tudo.

**Um agente de pentest que vive no seu terminal.** Você fala em português, ele pensa, escreve, executa e prova. Recon, exploração, auditoria de projeto, relatórios — sem sair da linha de comando.

</div>

---

## O que é o clawfast

Imagine abrir o terminal, digitar um pedido em linguagem natural e ver um operador de segurança trabalhar de verdade na sua frente: rodando comandos, fazendo requisições HTTP, mapeando alvos, encontrando vulnerabilidades e escrevendo o laudo. Não é autocomplete. É um **agente autônomo** com ferramentas reais nas mãos.

O clawfast roda **na sua máquina**, com **suas próprias chaves de modelo**, **sem login** e **sem limite de uso**. O que você manda fazer, ele faz.

> Você comanda. Ele executa. O terminal vira sua arma.

---

## Instalação

Um comando. É tudo.

```bash
npm install -g clawfast
```

Pronto — o comando `clawfast` agora existe em qualquer pasta do seu sistema.

Requisito único: **Node.js 18+**. Funciona em Windows, Linux e macOS.

---

## Primeiro contato

Abra o terminal e digite:

```bash
clawfast
```

Na primeira vez, ele pede **uma chave de modelo** — só isso. Use a [NVIDIA build](https://build.nvidia.com/), que dá acesso a **modelos de ponta gratuitos**:

1. Crie a chave em **https://build.nvidia.com/**
2. Cole quando o clawfast pedir.

A chave fica salva com segurança em `~/.clawfast/.env`. A partir daí, é só digitar `clawfast` e começar — ele já lembra de você.

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

Ele decide as ferramentas, executa comando por comando, mostra a saída em tempo real e entrega o resultado. Você acompanha tudo na tela — e pode interromper, redirecionar ou aprofundar a qualquer momento.

### Controles enquanto ele trabalha

| Tecla | O que faz |
|-------|-----------|
| `Ctrl+C` (1x) | interrompe a tarefa atual |
| `Ctrl+C` (2x em 2s) | fecha o clawfast |
| `↑` / `↓` | navega pelo histórico de comandos |
| digitar durante a execução | manda texto direto para o programa em andamento (REPLs, prompts, `sudo`…) |

---

## Comandos de barra

Digite `/` a qualquer momento para abrir o menu ao vivo. Os poderes ficam a um atalho de distância:

| Comando | O que faz |
|---------|-----------|
| `/model` | troca o modelo da sessão num seletor com setas |
| `/api` | troca sua chave NVIDIA na hora — testa, valida e ativa sem reiniciar |
| `/skills` | lista as skills instaladas |
| `/skillcreator` | cria uma nova skill (ensina o clawfast a fazer algo do seu jeito) |
| `/system` | salva o cérebro do agente (system prompt) num visualizador HTML |
| `/nov` | mostra as novidades desta versão |
| `/exit` | fecha o clawfast |

---

## Usando dentro de um projeto

Entre na pasta do seu projeto e rode `clawfast` ali mesmo. Peça uma auditoria e ele **vira um auditor dedicado, somente-leitura** — sozinho, sem comando especial:

```text
❯ analise o meu projeto e procure vulnerabilidades e segredos
```

A partir daí, ele:

- **Mapeia o projeto inteiro** — grafo de imports nos dois sentidos, achando imports quebrados, ciclos e arquivos órfãos.
- **Caça vulnerabilidades de verdade** — SQLi, injeção de comando/código, XSS, TLS desligado, cripto fraca, CORS aberto, chaves e tokens embutidos (mascarados no laudo).
- **Orquestra ferramentas de ponta** — combina análise determinística com o raciocínio do modelo para rastrear o fluxo de dados de ponta a ponta.
- **Só aceita o que pode provar** — descarta achado sem caminho confirmado no código real. Sem ruído, sem falso positivo inventado.

E o mais importante: nesse modo ele **nunca edita, cria ou apaga nada** no seu projeto. A única coisa que ele escreve é **um relatório** `ANALISE_PROJETO_<data>.md` na raiz. Seu código fica intocado.

Quer voltar ao modo de ataque normal? É só dizer: **"modo normal"**.

---

## Acessando sites e alvos

O clawfast não é teórico — ele **fala com a rede**:

- **Requisições HTTP nativas** — GET, POST, headers, cookies, payloads. Ele bate no alvo e lê a resposta.
- **Recon de superfície completo** — descoberta de subdomínios, varredura de portas, coleta de conteúdo e inteligência sobre o alvo.
- **Execução real** — roda os scanners e ferramentas que você tem instalados (e te ajuda a instalar os que faltam).
- **Verificação de exploit** — não basta "parece vulnerável": ele monta a prova e executa para confirmar.

Tudo dentro de um **controle de escopo**: você define o alvo, ele respeita a fronteira. Poder com responsabilidade.

> Use o clawfast apenas em sistemas que você tem **autorização explícita** para testar.

---

## Ensine novos truques: Skills

O clawfast aprende. Com `/skillcreator` você cola ou escreve uma **skill** — uma técnica, um checklist, um playbook do seu jeito — de qualquer tamanho:

```text
❯ /skillcreator
nome da skill?      ❯ xss-recon
descrição curta?    ❯ rotina de caça a XSS refletido e armazenado
palavras-gatilho?   ❯ xss, cross-site scripting
cole a skill agora  ❯ (cole o conteúdo... termine com /fim)
```

A skill vira **conhecimento disponível para todos os modelos** — mas o conteúdo completo só é carregado quando o seu pedido casa com ela. Eficiente e sempre pronto. Liste com `/skills`, remova com `/skill delete <nome>`.

---

## Escolha o cérebro: Modelos

O clawfast tem uma **cadeia de fallback automática** — se um modelo falha ou bate em limite, ele passa para o próximo sozinho, sem você perceber. Resiliência a rate limit (429) embutida.

Provedor primário: **NVIDIA build** (gratuito), com modelos de função de verdade — as ferramentas disparam de fato:

- `mistralai/mistral-medium-3.5`
- `openai/gpt-oss-120b`
- `z-ai/glm-5.1`
- `qwen/qwen3.5-397b`

Quer fixar um modelo? Abra `/model` e escolha com as setas. Quer voltar ao automático? `/model auto`. Opcionalmente, configure também sua própria chave OpenAI.

---

## Mantenha-se na frente: Update

O clawfast **avisa quando há versão nova** assim que você abre. Atualizar é um comando:

```bash
clawfast update
```

Ele se atualiza sozinho e te dá as boas-vindas de volta. Depois, rode `/nov` para ver tudo que mudou.

Quer só conferir a versão atual?

```bash
clawfast --version
```

---

## Referência rápida

```bash
npm install -g clawfast      # instala
clawfast                     # abre o agente
clawfast update              # atualiza para a última versão
clawfast --version           # mostra a versão instalada
```

Dentro do agente:

```text
/model   /api   /skills   /skillcreator   /system   /nov   /exit
```

---

<div align="center">

### O terminal sempre foi seu. Agora ele trabalha por você.

**Com o clawfast você faz tudo.**

</div>
