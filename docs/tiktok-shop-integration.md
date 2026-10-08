# Integração oficial com TikTok Shop

Esta versão da Central TikTok inicia em modo demonstração. O front-end já separa a origem dos dados e está preparado para trocar os exemplos por respostas da integração autorizada.

Na operação normal, a interface não mostra os exemplos. Eles só ficam acessíveis quando o usuário abre explicitamente o modo de prévia com `?demo=1`.

## O que a API oficial permite

O Partner Center documenta endpoints para produtos, busca de produtos, desempenho de produtos, vendedores e integrações de afiliados. O acesso é condicionado à aplicação aprovada, à região e aos escopos autorizados. As APIs de desempenho de loja exigem um token OAuth de vendedor e os dados de analytics podem ter latência T-1; por isso a interface precisa mostrar a origem e o horário de cada dado.

- [TikTok Shop Partner Center — desempenho de produtos](https://partner.tiktokshop.com/docv2/page/kpkfccsa)
- [TikTok Shop Partner Center — buscar produtos](https://partner.tiktokshop.com/docv2/page/search-products-202309)
- [TikTok Shop Partner Center — integração de afiliados](https://partner.tiktokshop.com/docv2/page/affiliate-integration)
- [TikTok Research API — produtos TikTok Shop](https://developers.tiktok.com/docs/en/research-api-specs-query-tiktok-shop-products)

Não existe um endpoint público e irrestrito que autorize este site a listar “tudo do TikTok Shop” sem uma conta/app com acesso. A aplicação deve trabalhar somente com os produtos, lojas e métricas que o token e os escopos concederem.

## Fluxo recomendado

1. Criar uma aplicação no portal oficial de parceiros do TikTok Shop.
2. Configurar o fluxo OAuth oficial, usando `state` e `nonce` para proteção contra CSRF e replay.
3. Solicitar apenas os escopos necessários para catálogo, pedidos, métricas, vendedores e afiliados.
4. Trocar o `authorization_code` no servidor. Nunca enviar client secret para o navegador.
5. Criptografar e armazenar tokens no servidor, com rotação e revogação controladas.
6. Normalizar cada resposta para o modelo interno de produtos, vendedores e métricas.
7. Guardar `source`, `updated_at` e `data_status` em todas as métricas. Quando o endpoint não fornecer uma métrica, usar `null` e renderizar “Não disponível”.

## Variáveis de ambiente sugeridas

```text
TIKTOK_SHOP_CLIENT_KEY=
TIKTOK_SHOP_CLIENT_SECRET=
TIKTOK_SHOP_REDIRECT_URI=https://seu-dominio.com/api/tiktok/callback
TIKTOK_SHOP_REGION=BR
```

## Endpoints de aplicação sugeridos

- `GET /api/tiktok/authorize` inicia OAuth e redireciona para a página oficial.
- `GET /api/tiktok/callback` valida `state`, troca o código e salva a conexão.
- `POST /api/sync/products` sincroniza catálogo e estoque.
- `POST /api/sync/metrics` sincroniza métricas permitidas.
- `GET /api/products` retorna os produtos normalizados com filtros e paginação.
- `GET /api/products/:id` retorna detalhes e histórico.
- `POST /api/products/:id/save` salva produto para o usuário autenticado.

## Regras de segurança

- Não solicitar nem armazenar senha do usuário.
- Não exibir access token ou refresh token na interface.
- Aplicar isolamento por usuário em todas as consultas.
- Respeitar escopos, rate limits, termos e políticas da API oficial.
- Registrar erros sem incluir tokens, dados pessoais ou payloads sensíveis.
- Validar a assinatura e a origem de webhooks, quando disponíveis.

Os nomes exatos dos endpoints, escopos e campos devem ser confirmados na documentação vigente do programa TikTok Shop Partner antes de habilitar a produção.
