# AI Context Hub

Repositório central para contextos de sessões de IA.

## Paradigma de uso

**1 Chat de IA = 1 Diretório Isolado.** Para alternar, utilize `ctx-sync load <nome_da_pasta>`.

## Comandos

```bash
ctx-sync init nome-da-sessao
ctx-sync save nome-da-sessao -m "descrição da alteração"
ctx-sync load nome-da-sessao
```

Cada sessão contém `01_CONTEXT.md`, `02_HISTORY.md` e `03_NEXT_STEPS.md`. O Git mantém o histórico e sincroniza as alterações com o GitHub.
