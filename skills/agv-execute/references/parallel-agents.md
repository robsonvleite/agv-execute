# Agentes em paralelo

Rodar tarefas ao mesmo tempo economiza relógio, mas custa tokens e pode gerar conflito. Use com critério.

## Quando paralelizar

Todas as condições, juntas:

- As tarefas **não mexem nos mesmos arquivos**.
- Nenhuma precisa do **resultado** da outra para começar.
- Cada uma tem **teste próprio** e dá para revisar separada.
- Cada uma é **grande o bastante** para valer um agente (não é ler um arquivo ou trocar uma linha).

Exemplos bons: três telas independentes; corrigir testes que falham em módulos diferentes; backend de uma
rota enquanto outra pessoa (agente) escreve a documentação de outra.

Exemplos ruins: duas tarefas no mesmo componente; "investigar" algo que você mesmo resolveria com um grep;
refatoração que toca o projeto inteiro.

## Como disparar

- Em geral **2 a 4 agentes** por rodada. Mais que isso, avise a pessoa do custo antes (uma linha basta).
- Dispare todos na **mesma mensagem**, para rodarem juntos.
- Use o modelo mais simples que dá conta para trabalho mecânico (renomear, formatar, traduzir); o modelo
  principal fica para decidir e para a parte difícil.
- Cada prompt segue o modelo de `task-execution.md`: autocontido, com arquivos permitidos, teste e o que
  devolver. Diga explicitamente que outros agentes estão mexendo em outras partes e que ele não deve tocá-las.
- Subagente **não usa git** (nem add, nem commit, nem checkout). Quem commita é você, no fim, conferindo o índice.

## Ao voltar

1. Leia o resumo de cada um.
2. Confira se houve conflito: `git status` e `git diff` mostram arquivo tocado por dois agentes?
3. Rode a suíte inteira (ou o build) uma vez, com tudo junto. Teste isolado verde não garante o conjunto.
4. Corrija a integração você mesmo ou com um agente novo e focado.
