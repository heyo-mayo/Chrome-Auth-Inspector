# Privacy Policy for Auth Inspector (Chrome DevTools Extension)

**Effective date:** 2025-08-13

Auth Inspector is a Chrome DevTools panel that helps you debug SAML and OIDC traffic. We designed it to work **entirely locally** in your browser and to avoid collecting or transmitting personal data.

---

## What the extension does
- Observes the **DevTools Network** activity of the **currently inspected tab** to display SAML/OIDC requests and responses (e.g., `SAMLRequest`, `SAMLResponse`, `/authorize`, `/token`).
- Parses and pretty-prints payloads **in memory** to show human-readable details (e.g., JWT claims, SAML assertions).
- Provides UI controls (filters, redaction, export/copy) to help with troubleshooting.

---

## Data collection & sharing
- **We do not collect, transmit, or sell any data.**
- **No telemetry or analytics.**
- **No external servers.** All processing happens locally within your browser.
- **No data is shared** with third parties.

---

## What we store (non-sensitive settings only)
We use `chrome.storage.local` to remember small UI preferences so the panel behaves consistently across sessions:
- Redaction on/off and pretty-print on/off  
- Last active detail tab (Parsed / Decoded / Raw)  
- Host/text filters and “Only this host” toggle  
- Pause state and other panel layout choices  
- Extension/version flags (e.g., “what’s new” seen)

You can clear these at any time via the panel’s reset/clear controls or by removing the extension.

---

## What we **do not** store
- **No SAML assertions or OIDC tokens**
- **No raw network payloads**
- **No browsing history or page content beyond the inspected tab’s DevTools network events**
- **No user identifiers, emails, or PII** (unless they appear in the inspected tab’s network traffic you choose to view—see “Your control” below)

---

## Clipboard usage
When you click “Copy” or “Export,” the extension copies **only what you explicitly requested** to the clipboard.  
- By default, **raw bearer tokens and other sensitive fields are redacted** in “Raw” views to reduce accidental disclosure.  
- You can choose to copy parsed/decoded information instead of raw strings.

The extension never writes to the clipboard without your action.

---

## Your control
- **Open/close DevTools** to start/stop viewing events.  
- Use **Pause** to stop capturing while DevTools remains open.  
- Apply **filters** (host/text) and **redaction** to limit what’s visible.  
- **Export/Copy** is manual and opt-in.  
- Remove the extension to delete all stored settings.

---

## Permissions rationale
- **DevTools**: required to add a panel and read the Network log for the inspected tab.  
- **Storage**: stores the non-sensitive preferences listed above.  
- *(No host permissions are requested.)*

---

## Security considerations
- Processing occurs **within Chrome DevTools** for the inspected tab; data is not persisted by the extension.  
- Redaction heuristics aim to hide bearer credentials by default.  
- If you capture or copy sensitive data, handle it according to your organization’s policies.

---

## Children’s privacy
This developer tool is intended for adults in professional contexts and does not knowingly collect information from children.

---

## Changes to this policy
We may update this policy as the extension evolves. Material changes will be reflected here with a new effective date.

---

## Contact
Questions or concerns? Please open an issue on the GitHub repository:  
**https://github.com/heyo-mayo/Chrome-Auth-Inspector/issues**

--- 

*Summary:* Auth Inspector processes authentication traffic **locally** to help you debug. It stores **only** UI preferences, never sends data to external services, and never collects analytics.

    f.write(content)
path
