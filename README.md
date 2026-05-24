# IR Relay — Wasmer Edge

HTTP Relay ported from Netlify Edge Functions to **Wasmer Edge JS Workers**.

## ساختار پروژه

```
wasmer-relay/
├── src/
│   └── index.js      ← منطق اصلی relay
├── public/
│   └── index.html    ← صفحه پیش‌فرض
├── wasmer.toml       ← تعریف package برای Wasmer
├── app.yaml          ← تنظیمات deploy روی Wasmer Edge
└── README.md
```

## نحوه deploy

### ۱. نصب Wasmer CLI
```bash
curl https://get.wasmer.io -sSfL | sh
```

### ۲. لاگین
```bash
wasmer login
```

### ۳. دیپلوی
```bash
cd wasmer-relay
wasmer deploy
```

بعد از deploy، آدرسی مثل `https://ir-relay.wasmer.app` دریافت میکنی.

## نحوه استفاده

برای relay کردن درخواست به یه هاست دیگه، هدر `x-host` رو بفرست:

```bash
# پروتکل خودکار (https)
curl https://ir-relay.wasmer.app/api/data \
  -H "x-host: api.example.com"

# با پروتکل صریح
curl https://ir-relay.wasmer.app/path \
  -H "x-host: http://internal-server:8080"
```

## تفاوت‌های Netlify → Wasmer

| Netlify | Wasmer Edge |
|---|---|
| `export default async function handler(request, context)` | `addEventListener("fetch", ...)` |
| `netlify.toml` | `wasmer.toml` + `app.yaml` |
| Deno runtime | JS Worker runtime (V8) |
| Edge Functions در `netlify/edge-functions/` | سورس در `src/` |
