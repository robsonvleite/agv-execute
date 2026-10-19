# Changelog

Todas as mudanças relevantes desta skill. Formato inspirado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/),
versões em [SemVer](https://semver.org/lang/pt-BR/).

## 1.1.1 · 2026-10-19

Correções vistas num teste lado a lado com outra skill de execução, na mesma tarefa.

- `plan.md` obrigatório sempre que a entrega mexe em dois arquivos ou mais, mesmo com o plano aprovado de antemão.
- Regra de projeto limpo: `git status` antes do relatório e nenhum arquivo temporário na raiz.
- Revisão dupla feita pelo próprio executor em entrega pequena (mais barato); subagente revisor só em entrega
  grande ou arriscada.
- Teste no navegador: conferir opção desconhecida no `--help` e apagar `verificacao/` no fim.

## 1.1.0 · 2026-10-19

- Revisão dupla por tarefa (`references/review.md`): primeiro escopo (fez o que foi pedido, nem mais nem menos),
  depois qualidade (bug, segurança, padrão do projeto, sobra), com modelos de prompt para o revisor e limite de
  duas rodadas.
- A execução por tarefa só marca `[x]` no plan.md depois das duas revisões.

## 1.0.0 · 2026-10-18

Primeira versão pública.

- Fase 0: confere git, Node.js e agent-browser; instala o navegador de teste com permissão.
- Fase 1: brainstorm com uma pergunta por vez até objetivo, critério de pronto e restrições.
- Fases 2 e 3: pendências na ordem certa e um único `plan.md` na raiz, com um único OK.
- Fase 4: execução por tarefa, com modelo de prompt para subagente e regras de paralelismo.
- Fase 5: testes, build e frontend no navegador em computador e celular.
- Fase 6: relatório curtíssimo com o que foi feito, o que falta e o que depende da pessoa.
