# Postfix SMTP Relay to Gmail using OAuth2

Configure Postfix on Debian/Ubuntu as a local SMTP relay that forwards mail through Gmail using OAuth2 (XOAUTH2) authentication.

## Why

Google has permanently disabled "Less Secure App" access. This guide uses the [sasl-xoauth2](https://github.com/tarickb/sasl-xoauth2) plugin so legacy clients (printers, monitoring tools, scripts, etc.) that only support plain SMTP can continue sending mail through Gmail.

```
Legacy Client --SMTP:25--> Postfix Relay --OAuth2:587--> smtp.gmail.com
```

## What's Covered

- Google Cloud Console setup (project, Gmail API, OAuth2 credentials)
- Building and installing the sasl-xoauth2 SASL plugin
- Postfix configuration (`main.cf`, `sasl_passwd`, TLS, chroot)
- OAuth2 token generation and automatic refresh
- Sender rewriting for Gmail's From-address enforcement
- Troubleshooting common errors
- Gmail sending limits and security recommendations

## Files

| File | Description |
|------|-------------|
| `postfix-gmail-oauth2.pdf` | Full guide (PDF) |
| `postfix-gmail-oauth2.html` | Guide source (HTML) |

## Quick Start

1. Create a Google Cloud project and enable the Gmail API
2. Create OAuth2 credentials (Desktop app) — note the Client ID and Secret
3. Install dependencies and build sasl-xoauth2
4. Configure Postfix with `relayhost = [smtp.gmail.com]:587` and XOAUTH2 SASL
5. Run `sasl-xoauth2-tool get-token gmail <token-path> --client-id <ID> --client-secret <SECRET> --scope https://mail.google.com/` to authorize
6. Restart Postfix and send a test email

Full step-by-step instructions are in the PDF/HTML guide.

## Requirements

- Debian 12 / Ubuntu 22.04+
- Gmail or Google Workspace account
- Google Cloud Console access
- Outbound connectivity to `smtp.gmail.com:587` and `oauth2.googleapis.com:443`
