# Fase 0 · Ambiente

Objetivo: garantir que dá para testar de verdade antes de prometer qualquer entrega.

## 1. Conferir (sem instalar nada)

Rode e anote o resultado de cada um:

```bash
git rev-parse --is-inside-work-tree   # é um repositório git?
node --version                         # Node.js (precisa para instalar o agent-browser via npm)
npm --version
agent-browser --version                # navegador de teste
agent-browser doctor                   # confere se o Chrome de teste está instalado
```

Descubra também se o projeto tem frontend web (procure `package.json` com next, vite, react, vue, svelte,
astro, ou arquivos `.html`). O agent-browser só é obrigatório quando há frontend. Num projeto só de backend
ou de scripts, ele é opcional: diga isso e siga sem ele se a pessoa preferir.

## 2. Se faltar algo: explicar, pedir permissão, instalar

Nunca instale sem permissão. Mostre uma mensagem curta assim:

> Para testar as telas no navegador eu uso o **agent-browser** (Vercel Labs, licença Apache-2.0): ele abre
> um Chrome de teste, clica, preenche e lê os erros da página. Não está instalado aqui. Vou rodar:
>
> ```
> npm install -g agent-browser
> agent-browser install
> ```
>
> O primeiro instala o comando; o segundo baixa o Chrome for Testing (só na primeira vez). Posso instalar?

Comandos por situação (conferidos no README oficial, github.com/vercel-labs/agent-browser):

| Situação | Comando |
|---|---|
| Padrão (npm) | `npm install -g agent-browser && agent-browser install` |
| macOS com Homebrew | `brew install agent-browser && agent-browser install` |
| Linux (bibliotecas do sistema) | `agent-browser install --with-deps` depois do npm |
| Já instalado, versão velha | `agent-browser upgrade` |

Se `agent-browser install` falhar, rode `agent-browser doctor --fix` e mostre a saída. Chrome, Brave,
Playwright ou Puppeteer já instalados são detectados automaticamente.

**Sem Node.js:** não instale Node por conta própria. Diga que precisa do Node.js (nodejs.org, versão LTS) ou
do Homebrew no Mac, e siga com o que dá para fazer sem o navegador. O teste de tela vira item "depende de você".

**Sem git:** pergunte se pode rodar `git init`. Sem git não há histórico para conferir o que já foi feito
nem como desfazer; avise isso em uma linha.

## 3. Confirmar

Depois de instalar, rode de novo `agent-browser --version` e `agent-browser doctor`. Só marque a fase como
concluída quando os dois responderem sem erro.

Para aprender o uso detalhado do navegador, o próprio CLI traz o guia na versão certa:
`agent-browser skills get core`. Use isso em vez de chutar comandos.
