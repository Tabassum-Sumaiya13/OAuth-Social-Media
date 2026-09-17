# Social Media API Integration Guide

A complete reference for integrating OAuth and posting to **Facebook, Instagram, LinkedIn, TikTok, and YouTube**.

## 📁 Contents

| File | Description |
|------|-------------|
| `Access_Token__Token_Durations_Refresh_Methods__Invalidation_Rules.pdf` | Token lifetimes, refresh flows, and invalidation rules per platform |
| `All_Errors_Encountered__Their_Fixes__flow.pdf` | Common errors, causes, fixes, and end-to-end flow summaries |
| `OAuth_for_Facebook.pdf` | Facebook OAuth setup + Page token flow |
| `OAuth_for_instagram_.pdf` | Instagram Graph API setup + media publishing |
| `OAuth_for_Linkdin.pdf` | LinkedIn OAuth setup + posting via `ugcPosts` |
| `OAuth_for_Tiktok.pdf` | TikTok OAuth setup + video upload/publish |
| `OAuth_for_Youtube.pdf` | Google Cloud + YouTube Data API setup + uploads |

## 🚀 Quick Start

Each platform guide follows the same structure:
1. **Prerequisites** – accounts, app types, roles
2. **App Setup** – products, scopes, redirect URIs
3. **OAuth Config** – Postman/code settings
4. **API Calls** – get IDs, publish content
5. **Errors & Fixes** – troubleshooting

## 🔑 Token Lifetimes (Cheat Sheet)

| Platform | Short-lived | Long-lived | Refresh |
|----------|-------------|------------|---------|
| Facebook User | ~1–2 hrs | 60 days | `fb_exchange_token` |
| Facebook Page | No fixed expiry* | — | Via `/me/accounts` |
| Instagram | 1 hr | 60 days | `ig_refresh_token` |
| TikTok | 24 hrs | 365 days (refresh) | Daily refresh |
| LinkedIn | 30 min (auth code) | 60 days | Partners only |
| YouTube | 1 hr | Refresh token never expires | `grant_type=refresh_token` |

\*Page tokens invalidate on password change, app revoke, or 90-day inactivity.

## ⚠️ Critical Gotchas

- **Facebook/LinkedIn**: Send credentials in **body**, not Basic Auth header.
- **Instagram**: Must use **Instagram Login** flow (not Facebook Login); account must be **Business/Creator**; images must be on a whitelisted CDN (e.g., Cloudinary).
- **TikTok**: Use `client_key`, not `client_id`. Sandbox limited to 20MB videos.
- **YouTube**: Include `access_type=offline` + `prompt=consent` to get a refresh token.
- **Never** store tokens client-side. Encrypt at rest. Regenerate if exposed.

## 🔒 Security Checklist

- [ ] Secrets in environment variables
- [ ] Tokens encrypted in DB / secret manager
- [ ] HTTPS redirect URIs in production
- [ ] Random `state` param validated on callback
- [ ] Refresh tokens rotated after use
- [ ] Credentials regenerated if leaked

## 🧪 Testing

All guides include **Postman** configurations using:
```
Callback URL: https://oauth.pstmn.io/v1/callback
```

## 📌 Flow Summary

```
Facebook:  Login → Short (1h) → Exchange → Long (60d) → /me/accounts → Page Token → Post
Instagram: Auth → Short (1h) → Exchange → Long (60d) → Refresh → New 60d window
TikTok:    Auth Code → Access (24h) + Refresh (365d) → Daily refresh
LinkedIn:  Auth Code (30m) → Access (60d) → Refresh (partners only)
YouTube:   OAuth (offline) → Access (1h) + Refresh (never) → Refresh on demand
```

## 📖 Usage

Read the platform-specific PDF for detailed step-by-step instructions. Start with `All_Errors_Encountered__Their_Fixes__flow.pdf` if you're debugging an existing integration.

## 📝 License

Internal reference documentation.
