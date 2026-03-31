# Berlin Group AIS Visualizer

A single-file browser tool that visualizes the **PSD2 / Berlin Group NextGenPSD2 v1.3** Account Information Service (AIS) consent creation flow step by step.

No server, no dependencies — just open `index.html` in a browser.

## What it does

Walks through the full TPP → ASPSP consent request flow:

1. **Generate Request Identity** — UUID + Date header
2. **Compute Body Digest** — real SHA-256 via Web Crypto API
3. **Build Signing String** — `digest · date · x-request-id`
4. **Sign with RSA-SHA256** — real PKCS#1 v1.5 signature via Web Crypto (when key is loaded)
5. **Assemble Headers** — Signature, Digest, TPP-Certificate, PSU headers

At the end it generates a complete, ready-to-run `curl` command.

## Usage

```bash
# Just open the file
open index.html
# or
xdg-open index.html
```

### Keys & Certificate

- Click **Generate demo keys** to create an RSA-2048 key pair and self-signed certificate in the browser — no tools required
- Or click **Load file** to pick your own `private_key.pem` / `certificate.pem` from disk, or paste the PEM content directly

When both are loaded the tool computes a real signature and fills the `TPP-Signature-Certificate` header — the generated output will be ready to run without any manual substitution.

> **Note:** Keys are never sent anywhere. All cryptographic operations run locally in the browser via the [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API).

### Modes

| Mode | Behaviour |
|---|---|
| **Auto Run** | Steps play through with animated delays |
| **Step by Step** | Pauses after each step — click **Run Step →** to advance |

### Download formats

After the flow completes, the request can be exported in several formats:

| Format | Description |
|---|---|
| **Copy** | Copies the curl command to clipboard |
| **Download curl** | Pre-signed snapshot — valid for ~5 min |
| **Download Bundle** | Bash script + key files — re-signs at runtime, always valid |
| **Download C#** | Self-contained .NET 6+ console app — re-signs at runtime |
| **Download Postman** | Postman collection — pre-signed snapshot |
| **Download .http** | VS Code REST Client / IntelliJ HTTP Client — pre-signed snapshot |

## Compatibility

Requires a modern browser with Web Crypto API support (Chrome 60+, Firefox 57+, Safari 11+, Edge 79+).

Private key must be in **PKCS#8 PEM** format (`-----BEGIN PRIVATE KEY-----`).

## Endpoint

Pre-configured for the [Open Bank Project](https://www.openbankproject.com/) sandbox:

```
POST https://apisandbox.openbankproject.com/berlin-group/v1.3/consents
```

Change the **Endpoint** and **Key ID** fields in the UI to target a different ASPSP.

## Security

- `private_key.pem` and `certificate.pem` are listed in `.gitignore` — do not commit them.
- The tool runs entirely client-side; no data leaves your machine.

## Troubleshooting

See [Troubleshooting.html](Troubleshooting.html) for a reference of common errors, their causes, and fixes.

## License

MIT
