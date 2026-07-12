# Akool (akool)

Akool is a generative AI platform for video and imagery - talking avatars, talking photos, face swap, video translation with lip-sync, background change, lip sync, image generation, and a real-time streaming (live) avatar product. The Akool OpenAPI exposes these tools as an HTTPS REST API under `https://openapi.akool.com`, authenticated with a Bearer token minted from a `clientId` / `clientSecret` pair (`POST /api/open/v3/getToken`) or a direct `x-api-key` header. Generation is asynchronous: callers create a task, then poll by id or receive an encrypted webhook callback when the task completes.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/akool/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/akool/refs/heads/main/apis.yml)

## Access Model (honest summary)

- **Public, documented REST API.** Akool publishes a full developer area at [docs.akool.com](https://docs.akool.com) (with an `llms.txt` index and several published OpenAPI files), a public [Postman collection](https://www.postman.com/akoolai/team-workspace/collection/ddsutn3/akool-official-apis), and a GitHub org at [AKOOL-Official](https://github.com/AKOOL-Official). This is a real, live API - not a modeled placeholder.
- **Base URL:** `https://openapi.akool.com`, with product endpoints under `/api/open/v3` and `/api/open/v4`.
- **Auth:** exchange `clientId` + `clientSecret` for a Bearer token at `POST /api/open/v3/getToken`, or pass a direct `x-api-key` header (Akool's recommended path). Tokens expire (error code `1101`).
- **Async by design:** create endpoints return a task id (`_id` / `job_id`); poll a status endpoint by id (`video_status` / `image_status` / `faceswap_status`: 1=queueing, 2=processing, 3=completed, 4=failed) or register a `webhookUrl` for an encrypted completion callback (AES-256-CBC `dataEncrypt`, SHA-1 `signature`).
- **API access is plan-gated.** Programmatic access is available on the higher subscription tiers (Pro Max and above per the pricing page); generation spends credits and generated assets are retained for 7 days.
- **The OpenAPI in this repo (`openapi/akool-openapi.yml`) is hand-modeled by API Evangelist** from the live documentation. Endpoint paths, methods, auth, and status semantics are grounded in Akool's docs; request/response field lists are representative rather than exhaustive. Akool also publishes its own per-product OpenAPI files (e.g. `https://docs.akool.com/openapi/live-avatar.yaml`) - consult those and the live docs for complete field detail. Plans, rate limits, and FinOps files are marked `reconciled: false` where exact prices/limits were not confirmed.
- **Real-time transport, stated precisely.** The Live Avatar (Streaming Avatar) product is real-time and bidirectional, but Akool does **not** expose an Akool-operated WebSocket API. The REST control plane opens a session (`POST /api/open/v4/liveAvatar/session/create`) whose media is carried over a caller-selected third-party WebRTC transport - `stream_type` `agora` (default), `livekit`, or `trtc` - using that provider's credentials and SDK. WebRTC media transport belongs to Agora/LiveKit/Tencent, not to a documented Akool WebSocket, so no AsyncAPI document was authored. See `review.yml`.

## Tags

- AI Avatars
- Video Generation
- AI Video
- Face Swap
- Generative AI
- Talking Avatar
- Video Translation
- Live Avatar

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### Akool Talking Avatar API

Generate an expressive, speaking avatar video from an Akool or user-supplied avatar plus text or an audio track. `POST /api/open/v3/talkingavatar/create` (avatar_id, voice_id, input_text or audio url), then poll by `_id`.

- **Human URL:** [https://docs.akool.com/ai-tools-suite/talking-avatar](https://docs.akool.com/ai-tools-suite/talking-avatar)
- **Base URL:** `https://openapi.akool.com`
- **Tags:** AI Avatars, Talking Avatar, Video Generation

### Akool Talking Photo API

Turn a still portrait into a talking video by pairing a `talking_photo_url` with an `audio_url`. `POST /api/open/v3/content/video/createbytalkingphoto`, poll `GET /api/open/v3/content/video/infobymodelid`.

- **Human URL:** [https://docs.akool.com/ai-tools-suite/talking-photo](https://docs.akool.com/ai-tools-suite/talking-photo)
- **Base URL:** `https://openapi.akool.com`
- **Tags:** AI Avatars, Talking Photo, Video Generation

### Akool Face Swap API

Swap a source face onto a target image or video, single-face or multi-face. `POST /api/open/v4/faceswap/faceswapPlusByImage`, results at `GET /api/open/v3/faceswap/result/listbyids`.

- **Human URL:** [https://docs.akool.com/ai-tools-suite/faceswap](https://docs.akool.com/ai-tools-suite/faceswap)
- **Base URL:** `https://openapi.akool.com`
- **Tags:** Face Swap, AI Video, Generative AI

### Akool Video Translation API

Translate a source video into one or more target languages with optional lip-sync. `GET /api/open/v3/language/list`, `POST /api/open/v3/content/video/createbytranslate`, then poll by `_id`.

- **Human URL:** [https://docs.akool.com/ai-tools-suite/video-translation](https://docs.akool.com/ai-tools-suite/video-translation)
- **Base URL:** `https://openapi.akool.com`
- **Tags:** Video Translation, AI Video, Lip Sync

### Akool Image Generation API

Text-to-image and image-to-image generation on Flux-family models. `POST /api/open/v4/content/image/createBySourcePrompt`, poll `GET /api/open/v3/content/image/infobymodelid`.

- **Human URL:** [https://docs.akool.com/ai-tools-suite/image-generate/image-generate](https://docs.akool.com/ai-tools-suite/image-generate/image-generate)
- **Base URL:** `https://openapi.akool.com`
- **Tags:** Generative AI, Image Generation, Text to Image

### Akool Live Avatar (Streaming) API

Real-time, interactive streaming avatars. REST control plane lists avatars (`GET /api/open/v4/liveAvatar/avatar/list`) and manages sessions (create/list/detail/close). Session media rides a caller-selected WebRTC transport (`stream_type` agora, livekit, or trtc) via `POST /api/open/v4/liveAvatar/session/create` - not an Akool WebSocket.

- **Human URL:** [https://docs.akool.com/ai-tools-suite/live-avatar](https://docs.akool.com/ai-tools-suite/live-avatar)
- **Base URL:** `https://openapi.akool.com`
- **Tags:** AI Avatars, Live Avatar, Real Time

## Common Properties

- [Authentication](authentication/akool-authentication.yml)
- [GitHub Organization](https://github.com/AKOOL-Official)
- [LinkedIn](https://www.linkedin.com/company/akoolai)
- [Website](https://akool.com)
- [Documentation](https://docs.akool.com)
- [Plans](plans/akool-plans-pricing.yml)
- [Rate Limits](rate-limits/akool-rate-limits.yml)
- [Fin Ops](finops/akool-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
