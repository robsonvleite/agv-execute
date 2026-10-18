# agv-execute · Modo Execução para o Claude Code

[![Licença: MIT](https://img.shields.io/badge/licen%C3%A7a-MIT-blue.svg)](LICENSE)
[![Versão](https://img.shields.io/github/v/release/robsonvleite/agv-execute?label=vers%C3%A3o)](https://github.com/robsonvleite/agv-execute/releases)

Você dá o objetivo. A IA entrega pronto.

O `agv-execute` é uma skill aberta para o [Claude Code](https://claude.com/claude-code) que transforma um
pedido em entrega testada: confere o ambiente, faz as perguntas certas (uma por vez), escreve o plano,
implementa, testa no navegador e só no fim te devolve um relatório curto com o que fez e o que depende de você.

É a versão em skill do prompt [Modo Execução](https://agenciadevalor.com/prompt/modo-execucao), que o Robson
V. Leite usa no dia a dia do Agência de Valor.

## Para quem é

- Quem já usa o Claude Code e cansou de responder "pode seguir?" a cada passo.
- Quem quer que o agente teste a tela de verdade antes de dizer que está pronto.
- Quem gosta de um `plan.md` simples na raiz, em vez de documentos espalhados.

Não é para quem quer aprovar cada linha de código: aqui você aprova o entendimento e o plano, e a skill executa.

## Instalação em 1 linha

```bash
npx skills add robsonvleite/agv-execute
```

Precisa de Node.js. O instalador ([skills.sh](https://github.com/vercel-labs/skills)) pergunta em qual agente e
em qual escopo instalar. Para instalar direto no Claude Code, para todos os projetos, sem perguntas:

```bash
npx skills add robsonvleite/agv-execute -g -a claude-code -y
```

Na primeira execução, a skill confere se o navegador de teste
[agent-browser](https://github.com/vercel-labs/agent-browser) está instalado. Se faltar, ela explica o que vai
instalar, pede sua permissão e roda `npm install -g agent-browser && agent-browser install`.

## Como usar

Abra o Claude Code na pasta do projeto e digite:

```
/agv-execute <seu objetivo>
```

Exemplo real de pedido:

```
/agv-execute criar a página de contato com formulário (nome, e-mail, mensagem) que envia para a rota /api/contact
```

Também funciona sem objetivo (`/agv-execute`): nesse caso ela termina o que estiver pendente no `plan.md`.

## O que acontece, passo a passo

| Fase | O que a skill faz | Você precisa |
|---|---|---|
| 0. Ambiente | Confere git, Node.js e o agent-browser. Instala o que faltar, com sua permissão. | Autorizar a instalação, se faltar algo |
| 1. Brainstorm | Lê o projeto e pergunta, uma coisa por vez, até fechar objetivo, critério de pronto e restrições. | Responder de 2 a 5 perguntas |
| 2. Pendências | Lê `plan.md`, regras do projeto (`CLAUDE.md`, `AGENTS.md`), histórico do git e código. Ordena o que falta. | Nada |
| 3. Plano | Escreve o `plan.md` na raiz com tarefas, arquivos, testes e o que depende de você. | Dar um único OK |
| 4. Execução | Implementa tarefa por tarefa, com subagentes em paralelo quando as tarefas são independentes. | Nada |
| 5. Verificação | Roda testes e build, abre as telas no navegador (computador e celular) e lê os erros da página. | Nada |
| 6. Relatório | Devolve o que fez e um checklist do que falta e do que depende de você. | Ler e fazer os itens seus |

Depois do OK do plano, a skill não para para perguntar. Ação destrutiva ou externa (deploy de produção,
apagar dados, disparar e-mail, `push --force`) nunca roda sozinha: vira item "depende de você", com o comando pronto.

### Como fica o relatório

```
## Feito
- Página /contato com formulário e validação (app/contato/page.tsx)
- Rota POST /api/contact com resposta 200 e 400 (app/api/contact/route.ts)
Verificado: testes passando, build ok, tela testada no navegador em 1440px e 390px, sem erro no console.

## Falta
- [ ] Nada.

## Depende de você
- [ ] Gerar a chave do provedor de e-mail e gravar em CONTACT_API_KEY
```

(Exemplo ilustrativo do formato, não de um projeto real.)

## O que tem dentro

```
skills/agv-execute/
  SKILL.md                    regras e as 6 fases
  references/
    setup.md                  conferência e instalação do ambiente
    brainstorm.md             como perguntar e fechar o entendimento
    plan.md                   pendências e formato do plan.md
    task-execution.md         execução por tarefa e prompt de subagente
    parallel-agents.md        quando e como rodar agentes em paralelo
    browser-testing.md        testes, build e frontend no navegador
    report.md                 o relatório final
```

A skill é autocontida: não exige o Superpowers nem outra skill instalada.

## Perguntas frequentes

**Funciona fora do Claude Code?**
Foi feita e testada para o Claude Code. Outros agentes que leem `SKILL.md` (Codex, Cursor e outros suportados
pelo `npx skills`) conseguem carregar, mas recursos como subagentes em paralelo dependem do agente.

**Ela commita ou faz deploy sozinha?**
Commit só se você autorizar no brainstorm. Deploy, push e qualquer ação externa, nunca: viram item seu no relatório.

**Preciso do agent-browser?**
Só se o projeto tem frontend web. Em projeto só de backend ou scripts, ele é opcional.

**Por que um único `plan.md`?**
Um arquivo na raiz, sempre atualizado, é fácil de ler, de versionar e de retomar em outra sessão. A skill
não cria planos espalhados.

**E se meu projeto já tem regras (`CLAUDE.md`, `AGENTS.md`)?**
Ela lê antes de planejar, e a regra do projeto vence a da skill.

**Gasta muito token?**
A skill evita desperdício: subagente só para trabalho grande e realmente paralelo, verificação em lote no fim,
e aviso antes de disparar mais de 4 agentes.

## Como atualizar

```bash
npx skills update agv-execute
```

As mudanças de cada versão estão no [CHANGELOG](CHANGELOG.md). A skill segue versionamento semântico
(`MAJOR.MINOR.PATCH`); a versão está no campo `version` do `SKILL.md`.

## Como contribuir

1. Abra uma [issue](https://github.com/robsonvleite/agv-execute/issues) contando o que aconteceu (o pedido, o
   que a skill fez e o que você esperava).
2. Para mudar a skill, faça um fork, edite os arquivos em `skills/agv-execute/` e abra um pull request.
3. Toda mudança sobe a versão no `SKILL.md` e ganha uma linha no `CHANGELOG.md`.
4. Regras de texto: português claro, sem travessão, frases curtas.

Se a skill te ajudou, deixe uma estrela no repositório: é o que ajuda outras pessoas a encontrarem.

## Inspiração e créditos

- [Superpowers](https://github.com/obra/superpowers), de Jesse Vincent (licença MIT): as ideias de brainstorm
  com uma pergunta por vez, plano em tarefas pequenas, execução com subagentes revisados e agentes em
  paralelo. O texto desta skill foi escrito do zero a partir dessas ideias; o aviso de licença está em
  [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
- [agent-browser](https://github.com/vercel-labs/agent-browser), da Vercel Labs (licença Apache-2.0): o
  navegador de teste. Não é distribuído aqui; a skill instala da fonte oficial com a sua permissão.
- [skills](https://github.com/vercel-labs/skills), da Vercel Labs (licença MIT): o instalador `npx skills`.

## Licença

[MIT](LICENSE) © 2026 Robson V. Leite · Agência de Valor

---

## English

**agv-execute** is an open skill for Claude Code that takes a goal to a tested delivery. It checks the
environment (installing the [agent-browser](https://github.com/vercel-labs/agent-browser) test browser with
your permission), runs a short brainstorm (one question at a time) to pin down the goal, the definition of done
and the constraints, writes a single `plan.md` at the project root, implements (with parallel subagents when
tasks are independent), tests everything including the frontend in a real browser, and ends with a very short
report: what was done and what still depends on you.

```bash
npx skills add robsonvleite/agv-execute
```

Then, inside Claude Code: `/agv-execute <your goal>`. The skill talks in your language; its instructions are
written in Portuguese. Self-contained: no other skill required. Inspired by
[Superpowers](https://github.com/obra/superpowers) (MIT). Licensed under MIT.
