# Fase 4 · Execução por tarefa

Siga o `plan.md` de cima para baixo. Para cada tarefa:

1. **Releia a tarefa** e os arquivos citados (só os trechos necessários).
2. **Escreva o teste primeiro quando der** (teste automatizado, script de verificação ou passo no navegador).
   Rode e veja falhar pelo motivo certo.
3. **Implemente o mínimo** que faz o teste passar, no estilo do código ao redor (nomes, comentários, padrões).
4. **Rode o teste** e veja passar. Rode também os testes vizinhos que podem ter quebrado.
5. **Revise o próprio diff** (`git diff`): sobrou código morto, segredo, console.log, arquivo que não devia?
6. **Marque `[x]` no plan.md.** Se a pessoa autorizou commit, commite só os arquivos desta tarefa.

## Delegar a um subagente

Vale a pena quando a tarefa é grande e autocontida, ou quando há tarefas paralelas
(veja `parallel-agents.md`). Não vale para ler um arquivo, procurar um símbolo ou conferir um fato.

O subagente não conhece a conversa. Entregue tudo no prompt:

```
Tarefa: <nome e objetivo em 1 frase>
Contexto: <projeto, stack, por que isso existe>
Arquivos que você pode mexer: <lista exata>. Não mexa em outros.
Requisitos: <lista verificável, copiada do plan.md>
Restrições do projeto: <regras do CLAUDE.md que importam aqui>
Como testar: <comando exato e resultado esperado>
Não faça: commit, push, deploy, instalar dependência nova sem dizer, apagar arquivos.
Devolva: o que mudou (arquivo por arquivo), a saída do teste e qualquer dúvida ou suposição.
```

## Revisar o que voltou

Nunca aceite o resultado de um subagente às cegas:

1. **Bate com a tarefa?** Fez tudo o que foi pedido, e nada além?
2. **Qualidade:** o diff segue o padrão do projeto? Tem gambiarra, duplicação, segredo?
3. **Rode o teste você mesmo.** A saída que o subagente relatou não é prova.

Se falhar, devolva ao mesmo subagente (ou a um novo) com o problema exato. Depois de duas tentativas sem
sucesso, pare essa tarefa, registre no plan.md e siga para as outras; ela entra no relatório.

## Quando travar

- **Erro que não entende:** leia a mensagem inteira, reproduza com o menor caso possível, forme uma hipótese
  e teste uma coisa por vez. Não troque três coisas de uma vez.
- **Falta algo da pessoa** (chave, acesso, decisão de produto): implemente tudo em volta, deixe o ponto pronto
  para ligar e registre em "Depende de você" com o passo exato.
- **O escopo cresceu no meio:** termine o que estava no plano, registre o novo no plan.md e avise no relatório.
