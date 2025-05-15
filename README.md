# TypeScript tsconfig.json: Por que `files` aceita `null`?

## Introdução

O `tsconfig.json` é o arquivo de configuração padrão do TypeScript. A propriedade `files` permite declarar explicitamente os arquivos a serem incluídos na compilação.

Porém, um detalhe interessante (e não documentado oficialmente) é que `files` também pode receber o valor `null`.

## Por que isso é permitido?

- JSON não permite `undefined`, mas permite `null`.
- O compilador do TypeScript normaliza `null` como se a propriedade não existisse.
- Isso evita erros desnecessários e torna o sistema mais tolerante e robusto.

## Comportamento

| Valor de `files`       | Comportamento                                |
|------------------------|----------------------------------------------|
| Ausente                | Inclui todos os arquivos exceto os excluídos |
| `null`                 | Mesmo comportamento que o ausente            |
| Lista de arquivos      | Compila apenas os arquivos listados          |

## Exemplo

Veja os arquivos no diretório `/src` e as configurações em:

- `tsconfig.valid.json`
- `tsconfig.null.json`

Teste com:

```bash
npx tsc -p tsconfig.valid.json
npx tsc -p tsconfig.null.json

## Referência

Esse comportamento está documentado no PR do TypeScript:  
[https://github.com/microsoft/TypeScript/pull/18058](https://github.com/microsoft/TypeScript/pull/18058)
