# client-server-v1-ss-gtm

Серверний Google Tag Manager (sGTM), розгорнутий на [Render.com](https://render.com) через Blueprint (`render.yaml`). Два сервіси: Preview Server і SST (tagging endpoint).

## Що потрібно в GTM

1. **Server container** у [Tag Manager](https://tagmanager.google.com/) (тип «Server»).
2. **Container Config**: відкрити серверний контейнер → зверху справа натиснути **Container ID** → **Manually provision tagging server** → скопіювати рядок **Container Config** (довгий base64-подібний). Він знадобиться для Render.

## Розгортання на Render

1. **Підключити репо** до Render: Dashboard → **New** → **Blueprint** → обрати репозиторій `client-server-v1-ss-gtm` (GitHub).
2. **Environment (секрети)** — для **обох** сервісів (`ss-gtm-preview` і `ss-gtm`):
   - Додати змінну **CONTAINER_CONFIG** (тип **Secret**).
   - Вставити рядок Container Config з GTM (з кроку вище).
3. **PREVIEW_SERVER_URL** — для сервісу **ss-gtm** у `render.yaml` за замовчуванням вказано `https://ss-gtm-preview.onrender.com`. Якщо Render призначить Preview-сервісу інший URL — у Dashboard для сервісу **ss-gtm** змінити змінну **PREVIEW_SERVER_URL** на фактичний HTTPS URL Preview.
4. Після деплою: у GTM → **Admin** → **Container Settings** → **Server container URL** вказати публічний URL сервісу **ss-gtm** (наприклад `https://ss-gtm.onrender.com`).

## Перевірка

- У GTM натиснути **Preview**, відкрити в іншій вкладці сторінку з **Server container URL**. У режимі Preview має з’явитися запит.
- Заливки тегів з сайту мають йти на URL сервісу **ss-gtm**.

## Важливо

- **CONTAINER_CONFIG** не комітити — тільки в Render Environment як Secret.
- Preview-сервіс має бути в одному інстансі (у `render.yaml` вже задано `minInstances: 1`, `maxInstances: 1`).
- Періодично перезапускати/редиплоїти сервіси, щоб отримувати оновлення образу `:stable`.
