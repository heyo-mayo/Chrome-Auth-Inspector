# Auth Inspector (SAML & OIDC)

A Chrome **DevTools** panel that helps you troubleshoot authentication flows with **Keycloak as a broker** (and any other IdP/SP). It captures and displays **SAML Requests/Responses** and **OIDC** `/authorize`, redirects, `/token` responses, and **fragment tokens** — with both **raw** payloads and **human-readable parsed views**.

All parsing happens **locally in your browser**. Nothing is sent off your machine.

---

## Features

- **DevTools panel** with three top filters: **All / OIDC / SAML**
- **Cards** for each captured event, collapsed by default
  - **Tabs** per card: **Parsed**, **Decoded**, **Raw** (one visible at a time)
  - **Copy** buttons for URL, parsed JSON, decoded XML/JWT, raw payload
- **SAML**
  - Supports **Redirect** and **POST** bindings
  - Pretty-printed XML
  - Parsed summary: Issuer, ID, Destination, Status, Signature algs, Assertions
  - Assertion details: Subject, Confirmation, Conditions, Audience, AuthnStatements, **Attributes** (lists show in dropdowns)
- **OIDC**
  - `/authorize` summary: client, redirect URI, response type/mode, scopes, **PKCE**, state/nonce presence
  - `/token` summary: token type, expiry, scopes, parsed **ID Token** and **Access Token** claims
  - **Roles** detection: Keycloak `realm_access` & `resource_access`, top-level `roles`/aliases, and **namespaced roles** (e.g., `https://example.com/roles`)
  - **User attributes**: organization/tenant/department/title/phone/locale (including namespaced keys)
- **Filters & UX**
  - Host filter, free-text search, “**This host**”, **Pause**, **Redact**, **Pretty**
  - **Export JSON** (redacted) of captured events
  - Lists render as compact **dropdowns**; single values are inline
- **Clipboard-safe**
  - Copy is triggered on click with a DevTools-friendly fallback (no background clipboard grabbing)

---

## How it works

- Listens to **Network** requests in the inspected tab (DevTools API).
- Detects SAML via `SAMLRequest`/`SAMLResponse` query/form fields and inflates/decodes XML if needed.
- Detects OIDC:
  - `/authorize` (query params),
  - redirects (302 `Location` with `code`/`state`),
  - `/token` (JSON body; decodes JWTs).
- **Fragment tokens** (`#id_token=...`) are captured by an optional, **on-demand** content script you can enable per-site (least-privilege).

---

## Installation (dev)

1. **Clone** this repo.
2. **Chrome → Manage Extensions** → toggle **Developer mode**.
3. Click **Load unpacked** and select the project folder (where `manifest.json` lives).
4. Open DevTools in any tab → **Auth Inspector** panel.

> Tip: after editing files, click **Reload** on the extension card and reopen DevTools.

---

## Permissions & Privacy

- **All parsing is local**. The extension does **not** transmit tokens/XML anywhere.
- **Permissions**
  - `storage`: save UI preferences (filters, redaction).
  - `scripting`: programmatically inject a tiny content script **only** when you click “Capture fragments”.
  - `activeTab` + `optional_host_permissions`: per-origin grant requested **on demand**.
  - `clipboardWrite` (optional): copy to clipboard **only** when you click “Copy”.
- **No remote code**: all JS/CSS is packaged with the extension.

If you use a public icon set (e.g., Material Symbols), see **Licensing** below.

---

## Usage

1. Open DevTools → **Auth Inspector**.
2. Pick a top tab (**All / OIDC / SAML**) if you want to narrow scope.
3. (Optional) Click **Capture fragments** to enable `#fragment` token capture for the current origin.
4. Run your auth flow. Cards will appear as requests happen.
5. Click a card header to expand. Use the **Parsed / Decoded / Raw** tabs as needed.
6. Use **Redact** to hide sensitive values when copying or exporting.

### What you’ll see

- **SAML**: Redirect vs POST, Issuer/ID/Destination/Status/Signature. Each Assertion shows Subject, Conditions, Audience, Authn statements, and **Attributes** (dropdown if multi-valued).
- **OIDC**:
  - **Authorize**: client, redirect URI, response type/mode, **PKCE** method, scopes, nonce/state presence.
  - **Token**: token type, expiry, scopes, **ID Token** & **Access Token** claims:
    - subject, issuer, times (auth/iat/exp),
    - groups,
    - **roles** (realm/client roles, top-level roles, namespaced roles),
    - **user attributes** (organization/tenant/department/title/phone/locale; namespaced supported).

---

## Troubleshooting

- **“No matching events yet”**  
  Make sure **Pause** is off, the **This host** toggle isn’t hiding events, and you’re filtering the correct tab (**All/OIDC/SAML**). Some flows do multiple redirects—keep the panel open while authenticating.

- **Clipboard error in DevTools**  
  Chrome can block the Async Clipboard API in DevTools pages. The app uses a safe `execCommand('copy')` fallback. If that still fails, you’ll see a prompt to copy manually.

- **Not seeing roles**  
  Many IdPs only put roles in the **access token**. Check the **Access Token Claims** section. This tool also detects top-level and namespaced roles (e.g., `https://example.com/roles`).

- **Fragment tokens not captured**  
  Click **Capture fragments** to inject the tiny listener for the current origin. Chrome will ask you to grant permission for that site.

---

## Known limitations (for now)

- **JWT/SAML signature verification** is not performed (parsing only).
- **Encrypted SAML assertions** aren’t decrypted.
- No automatic fetching of discovery/metadata/JWKS (by design—privacy first).

---

## Contributing

Issues and PRs welcome! If you contribute code:
- Keep everything **packaged** (no remote scripts/styles).
- Don’t add network calls that send token contents anywhere.
- Follow the code style in `panel/` modules (plain ES modules, no bundler required).

---

## Security

Please don’t attach real tokens or SAML assertions to issues. If you need to share a sample, redact or synthesize values. For private disclosures, open a minimal issue and we’ll coordinate.

---

## License

- **Code:** Apache 2.0 (see `LICENSE`).
- **Icons:**
  ```
  This product includes Material Symbols by Google,
  licensed under the Apache License, Version 2.0.
  https://developers.google.com/fonts/docs/material_symbols
  ```

---

## Roadmap (later, maybe)

- Flow correlation & mini timeline (state/RelayState/nonce/sid).
- Expiry/skew warnings for SAML `NotOnOrAfter` and JWT `exp`.
- Optional local verification (JWKS/certs) behind a user action.
- Compare runs (diff attributes/claims between two flows).

---



