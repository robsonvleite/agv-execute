# Revisão dupla por tarefa

Toda tarefa entregue (por você ou por um subagente) passa por duas revisões, nesta ordem. A segunda só começa
quando a primeira aprova.

Quem revisa depende do tamanho:

- **Entrega pequena** (até uns 3 arquivos, sem risco de segurança ou dado): faça você mesmo, relendo o diff do
  zero com o checklist de cada revisão abaixo, como se o código fosse de outra pessoa. Sai mais barato.
- **Entrega grande ou arriscada** (muitos arquivos, autenticação, pagamento, dado de usuário, migração): use um
  subagente revisor novo para cada revisão, com os prompts abaixo.

Tarefa trivial (uma linha, sem risco) dispensa a revisão formal: basta reler o diff e rodar o teste.

## 1. Revisão de escopo: fez o que foi pedido?

Pergunta única: a entrega bate com a tarefa do `plan.md`, nem mais, nem menos?

Prompt para o revisor:

```
Você revisa se uma entrega cumpre o pedido. Não avalie estilo nem qualidade de código.

Pedido (copiado do plan.md):
<tarefa, requisitos, critério de pronto e restrições>

O que foi entregue:
<diff completo ou lista de arquivos alterados para você ler>

Como o implementador disse que testou:
<comando e saída relatada>

Leia o código de verdade; não confie no relato. Responda neste formato:
VEREDITO: APROVADO ou REPROVADO
FALTOU: <cada requisito não atendido, com o arquivo e o motivo> (ou "nada")
SOBROU: <o que foi feito sem ter sido pedido> (ou "nada")
QUEBROU RESTRIÇÃO: <restrição violada e onde> (ou "nada")
```

Reprovou: devolva ao implementador só os itens de FALTOU, SOBROU e QUEBROU RESTRIÇÃO e revise de novo.

## 2. Revisão de qualidade: está bem feito?

Só depois do escopo aprovado. Pergunta: alguém do time aceitaria esse código sem retrabalho?

Prompt para o revisor:

```
Você revisa a qualidade de uma entrega que já cumpre o pedido. Não discuta o escopo.

Contexto do projeto: <stack e regras do CLAUDE.md/AGENTS.md que importam>
Diff: <diff completo>

Procure, nesta ordem de gravidade:
1. Bug: caso que quebra (entrada vazia, nula, longa demais, erro de rede, clique duplo, tela pequena).
2. Segurança: segredo no código, entrada sem validação, dado sensível em log.
3. Padrão do projeto: nome, estrutura e estilo diferentes do código ao redor; duplicação do que já existe.
4. Sobra: código morto, console.log, comentário velho, arquivo temporário.

Responda neste formato:
VEREDITO: APROVADO ou CORRIGIR
PROBLEMAS: <gravidade (bug, segurança, padrão, sobra) · arquivo:linha · o que acontece · como corrigir>
Liste só o que é real e verificável. Não aponte preferência pessoal.
```

CORRIGIR com bug ou segurança: corrija e revise de novo. Só padrão ou sobra: corrija e siga sem nova rodada.

## Limites

- No máximo duas rodadas por revisão. Na terceira reprovação, pare a tarefa, registre o motivo no `plan.md`
  e leve para o relatório.
- Revisão não substitui teste: rode o teste depois de qualquer correção.
- Em lote paralelo, revise cada tarefa separada e, no fim, rode a suíte inteira com tudo junto.
