# Central TikTok

Console responsivo de inteligência para TikTok Shop: produtos em alta, análise de oportunidades, vendedor original, produtos salvos, comparador, exportação e prompts para criativos.

## Estado atual

- A interface roda em modo demonstração, com os dados identificados como `Demonstração`.
- Produtos salvos, listas, preferências de tema e rascunhos são persistidos no navegador nesta primeira versão.
- A integração oficial está preparada na página **Integração TikTok Shop** e documentada em [`docs/tiktok-shop-integration.md`](./docs/tiktok-shop-integration.md).
- O app não solicita senha e não mostra tokens.

## Executar localmente

Como o front-end é estático, sirva a pasta `dist` com qualquer servidor HTTP. Exemplo:

```bash
python -m http.server 4173 --directory dist
```

Abra `http://localhost:4173`.

## Próximo passo de produção

Adicionar uma camada de servidor com autenticação, D1/R2 ou banco equivalente e o fluxo OAuth oficial do TikTok Shop. A UI já diferencia métricas confirmadas, estimadas, importadas, indisponíveis e demonstrativas, para que a fonte real seja exibida sem inventar valores.
