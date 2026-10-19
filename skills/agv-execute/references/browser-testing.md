# Fase 5 · Verificação

Rode tudo em lote, uma vez, no fim do bloco de trabalho. Corrija o que aparecer e confirme no máximo mais uma vez.

## 1. O que o projeto já tem

Procure no `package.json` (ou equivalente) e rode o que existir: testes (`test`), tipos (`typecheck`,
`tsc --noEmit`), lint e build. Mostre a saída resumida. Falhou: corrija antes de seguir.

Cuidado: build verde pode vir de cache. Se a mudança não aparece, limpe o cache do framework e confira o
arquivo gerado.

## 2. Frontend no navegador (agent-browser)

Suba o servidor de desenvolvimento em segundo plano (ex.: `npm run dev`) e espere responder. Então:

```bash
agent-browser open http://localhost:3000/rota
agent-browser snapshot            # árvore da página com refs (@e1, @e2...) para clicar e preencher
agent-browser errors              # erros de JavaScript da página: precisa estar vazio
agent-browser console             # avisos e logs
agent-browser screenshot ./verificacao/desktop.png   # o caminho vem logo depois do comando
```

Para cada critério de "pronto quando" que é visual ou interativo:

1. Faça o caminho que a pessoa faria: `click @eN`, `fill @eN "texto"`, `press Enter`, `wait 1000`.
2. Confira o resultado com `snapshot` ou `get text <seletor>`.
3. Leia `errors` depois de cada ação importante.

Teste também no tamanho de celular:

```bash
agent-browser set viewport 390 844
agent-browser reload
agent-browser screenshot ./verificacao/mobile.png
```

Olhe as capturas: texto cortado, rolagem horizontal, botão fora da tela, sobreposição. Feche no fim com
`agent-browser close`.

Comandos completos e padrões de uso, na versão instalada: `agent-browser skills get core`.

## 3. O que conta como verificado

- Cada critério de "pronto quando" tem uma prova: saída de comando, texto lido da página ou captura.
- `agent-browser errors` vazio nas telas mexidas.
- Nada quebrou fora do escopo (suíte inteira ou build passou).

Se algo não pôde ser verificado (falta acesso, serviço externo, dado real), diga isso no relatório. Não
escreva "funciona" sobre o que não rodou.

Antes de rodar um comando com opção que você não conhece, confira em `agent-browser --help`: opção errada pode
virar nome de arquivo (ex.: uma captura salva como `--width`).

No fim, apague a pasta `verificacao/` e qualquer arquivo temporário que não deve ir para o repositório, e confira
com `git status` que só sobraram as entregas.
