# Integração oficial com TikTok Shop

Esta versão da Central TikTok inicia em modo demonstração. O front-end já separa a origem dos dados e está preparado para trocar os exemplos por respostas da integração autorizada.

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
