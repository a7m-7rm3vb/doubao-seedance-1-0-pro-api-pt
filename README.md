# Seedance 1.0 Pro API (`seedance-1.0-pro`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0184 per second** on the APIMart route (billed by resolution tier) — um único endpoint compatível com OpenAI em `https://api.apimart.ai/v1`, sem plano mensal. *(observado em 2026-10-12)*

**[Obter uma chave de API](https://go.apimart.ai/k-d0d242)** · **[Preços ao vivo](https://go.apimart.ai/k-e669cf)** · **[Página do modelo](https://go.apimart.ai/k-39cfb1)** · [⚡ Início rápido em 60 segundos](#quickstart)

**Por que os times usam Seedance 1.0 Pro (`seedance-1.0-pro`) via APIMart**

- **Uma chave, o catálogo inteiro.** A mesma base URL `https://api.apimart.ai/v1` e o mesmo cabeçalho `Authorization` alcançam Seedance 1.0 Pro (`seedance-1.0-pro`) e mais de 300 modelos de imagem, vídeo e linguagem — troque o campo `model`, não o seu cliente.
- **Mínimo de $1, pague conforme o uso.** Sem assinatura e sem plano pré-pago para dimensionar antes: recarregue a partir de $1 e gaste em chamadas.
- **A cobrança volta na resposta.** Toda tarefa concluída informa o valor cobrado (`cost` / `credits_cost`); uma tarefa marcada como `failed` é reembolsada.
- **Assíncrono por design.** Envie, pegue o `task_id`, consulte `GET /v1/tasks/{id}` — batching e retries são trabalho comum de fila, não uma integração sob medida.

<!-- /conv-kit:v1 -->

**Também conhecido como:** Dreamina

Seedance 1.0 Pro is ByteDance's fast AI video model for high-quality video generation and rapid previews.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `seedance-1.0-pro` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-39cfb1) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-e669cf) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-39cfb1) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"seedance-1.0-pro","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "seedance-1.0-pro", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Qual é o id do modelo Seedance 1.0 Pro?**

`seedance-1.0-pro`. Veja a tabela de rotas acima.

**Como é a cobrança?**

**from $0.0184 per second** on the APIMart route (billed by resolution tier). A tarefa concluída informa o valor exato no campo `cost`; uma tarefa com falha é reembolsada.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-d0d242) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [doubao-seedance-1-0-pro-prompts-pt](https://github.com/a7m-7rm3vb/doubao-seedance-1-0-pro-prompts-pt)
- **Same model, other languages** — 9 more language builds: 中文 [doubao-seedance-1-0-pro-api-cn](https://github.com/utp5ty/doubao-seedance-1-0-pro-api-cn) · Deutsch [doubao-seedance-1-0-pro-api-de](https://github.com/ok4o3215xekjz/doubao-seedance-1-0-pro-api-de) · Español [doubao-seedance-1-0-pro-api-es](https://github.com/x3yprdl/doubao-seedance-1-0-pro-api-es) · Français [doubao-seedance-1-0-pro-api-fr](https://github.com/g30u5sa/doubao-seedance-1-0-pro-api-fr) · Relay/gateway [doubao-seedance-1-0-pro-api-gateway](https://github.com/i66153lsgst3/doubao-seedance-1-0-pro-api-gateway) · Bahasa [doubao-seedance-1-0-pro-api-id](https://github.com/pcrgss12wsx7w9/doubao-seedance-1-0-pro-api-id) · 日本語 [doubao-seedance-1-0-pro-api-ja](https://github.com/gy5vsnk744ehf8/doubao-seedance-1-0-pro-api-ja) · 한국어 [doubao-seedance-1-0-pro-api-ko](https://github.com/o4zjnjqk5zvm29/doubao-seedance-1-0-pro-api-ko) · Русский [doubao-seedance-1-0-pro-api-ru](https://github.com/nalaopn6vj/doubao-seedance-1-0-pro-api-ru)
- **More models in this language** — [midjourney-api-pt](https://github.com/l-ca0/midjourney-api-pt) · [gemini-2-5-flash-image-preview-api-pt](https://github.com/ycx8l0/gemini-2-5-flash-image-preview-api-pt) · [nano-banana-api-pt](https://github.com/erujt71i1lx/nano-banana-api-pt) · [gemini-3-1-flash-image-preview-api-pt](https://github.com/hutjwux0nt9h/gemini-3-1-flash-image-preview-api-pt) · [nano-banana-2-api-pt](https://github.com/ydkd8kdkl8n/nano-banana-2-api-pt) · [gemini-3-pro-image-preview-api-pt](https://github.com/qss2d-66qb2n7/gemini-3-pro-image-preview-api-pt)
- **Topic hubs** — every build of this model, all languages: [doubao-seedance-1-0-pro](https://github.com/topics/doubao-seedance-1-0-pro) · [doubao-seedance-1-0-pro-api](https://github.com/topics/doubao-seedance-1-0-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-39cfb1> | `apimart.ai/pt/model/doubao-seedance-1-0-pro` |
| Live pricing | <https://go.apimart.ai/k-e669cf> | `apimart.ai/pt/pricing` |
| Get an API key | <https://go.apimart.ai/k-d0d242> | `apimart.ai/pt/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedance 1.0 Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
