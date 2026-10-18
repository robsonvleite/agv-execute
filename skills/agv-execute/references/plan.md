# Fases 2 e 3 · Pendências e plano

## Fase 2: levantar as pendências na ordem certa

Fontes, nesta ordem:

1. `plan.md` da raiz (se existir): o que está marcado `[ ]` é pendente; `[x]` está feito.
2. `git log --oneline -30` e `git status`: o que já foi feito e o que está pela metade.
3. Regras do projeto: `CLAUDE.md`, `AGENTS.md`, `CONTRIBUTING.md`, README.
4. O código ligado ao objetivo: procure com `grep`/busca antes de abrir arquivos inteiros.
5. Marcadores no código (`TODO`, `FIXME`) só se ligados ao objetivo.

Ordene pelo que destrava o resto: dependência antes de quem depende, base de dados antes de tela, o que
tem risco técnico antes do que é rotina. Separe já o que depende da pessoa (chave, decisão, acesso, deploy).

## Fase 3: escrever o plan.md

Um arquivo só, na raiz, enxuto, focado no que falta. Formato:

```markdown
# Plano · <objetivo em uma linha>

Atualizado em <data>.

## Pronto quando
- [ ] <critério verificável 1>
- [ ] <critério verificável 2>

## Restrições
- <uma por linha, copiadas do brainstorm>

## Tarefas
- [ ] 1. <nome> · arquivos: `caminho/a`, `caminho/b` · teste: <como provar que funciona>
- [ ] 2. <nome> · depende de 1 · arquivos: ... · teste: ...
- [ ] 3. <nome> · paralela com 2 · arquivos: ... · teste: ...

## Depende de você
- [ ] <o que só a pessoa pode fazer, com o comando ou link pronto>

## Decisões tomadas
- <data> · <decisão> · <motivo em meia linha>
```

Regras de uma boa tarefa:

- **Tamanho:** a menor unidade que tem seu próprio teste. Configuração e documentação entram na tarefa que
  precisa delas, não viram tarefa à parte.
- **Arquivos exatos** e **como testar** em cada tarefa. Sem isso, o subagente chuta.
- **Marque paralelismo:** duas tarefas são paralelas só se não mexem nos mesmos arquivos nem dependem do
  resultado uma da outra.
- **Nada de reticência:** "tratar erros" não é tarefa; "mostrar mensagem X quando a API responder 401" é.

Se já existe um `plan.md` com outro assunto, não apague: acrescente uma seção nova no topo e preserve o resto.

## O único OK

Mostre o plano em poucas linhas (tarefas e o que depende da pessoa) e pergunte se pode executar. Com o OK,
você não volta mais a perguntar até o relatório. Mudança pequena de rota vira linha em "Decisões tomadas".

Durante a execução, atualize o `plan.md` a cada tarefa concluída (`[x]`). Ao terminar, remova o que foi feito
ou resuma em uma linha, deixando no arquivo só o que ainda falta.
