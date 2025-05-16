# TypeScript: Por que `files` (e outras propriedades) aceitam `null`?

## Introdução

No arquivo `tsconfig.json`, propriedades como `files`, `include`, `exclude` e `references` controlam quais arquivos o compilador TypeScript deve considerar.

Um comportamento curioso (e não oficialmente documentado) é que essas propriedades podem receber o valor `null` — e o TypeScript trata isso de forma silenciosa e tolerante.

## Por que `null` é aceito?

- O padrão JSON **não permite `undefined`**, mas permite `null`.
- O compilador **trata `null` como se a propriedade estivesse ausente**.
- Isso evita erros em arquivos gerados automaticamente ou malformados.
- Propriedades `null` são normalizadas internamente para `undefined`.

## Comportamento prático

| Propriedade       | Valor `null` permitido | Equivale a ausência? |
|-------------------|-------------------------|-----------------------|
| `files`           | ✅                      | ✅                    |
| `include`         | ✅                      | ✅                    |
| `exclude`         | ✅                      | ✅                    |
| `references`      | ✅                      | ✅                    |
| `extends`         | ❌ (gera erro)          | ❌                    |

## Exemplo com `files`

### `tsconfig.valid.json`

```json
{
  "compilerOptions": {
    "strict": true
  },
  "files": ["src/index.ts"]
}
````

### `tsconfig.null.json`

```json
{
  "compilerOptions": {
    "strict": true
  },
  "files": null
}
```

### Estrutura de arquivos

```bash
.
├── src
│   └── index.ts
├── tsconfig.valid.json
└── tsconfig.null.json
```

### Teste

```bash
npx tsc -p tsconfig.valid.json
npx tsc -p tsconfig.null.json
```

Nos dois casos, o TypeScript compila corretamente.

## Conclusão

Permitir `null` nas propriedades do `tsconfig.json` é uma decisão pragmática do compilador TypeScript para lidar com entradas malformadas sem falhar.

Esse comportamento **não está documentado na documentação oficial**, mas está descrito no PR:

🔗 [https://github.com/microsoft/TypeScript/pull/18058](https://github.com/microsoft/TypeScript/pull/18058)
