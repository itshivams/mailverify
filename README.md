<div align="center">
  <h1>🛡️ email-intel</h1>
  <p><b>The Ultimate Enterprise-Standard Email Verification & Intelligence Library</b></p>
  <p>Available for <b>JavaScript / TypeScript</b>, <b>Go</b>, and <b>Python</b>.</p>
</div>

---

## What is `email-intel`?

`email-intel` is an incredibly fast, robust, and deep email verification suite designed to run anywhere. It provides critical intelligence on email addresses without ever sending an email. 

**Primary Capabilities:**
1. **Valid Email Verification**: Probes MX records in real-time to guarantee the domain can receive emails.
2. **Disposable & Temp Email Detection**: Blocks thousands of "burn-after-reading" temporary email services (like 10minutemail, GuerrillaMail).
3. **Provider Inference**: Detects backend providers (Google Workspace, Microsoft 365, Zoho, Proofpoint, Mimecast, etc.).
4. **Domain Intelligence**: Accurately classifies domains into `Business`, `Public Webmail`, `Education`, `Government`, or `Organization`.
5. **DNS Auditing**: Validates the presence of SPF, DKIM, and DMARC security records.

---

## 🚀 Quick Start

`email-intel` maintains 100% feature parity across all 3 supported languages. Choose your stack below to get started!

### 🟡 JavaScript / TypeScript
Designed for Node.js backends and Frontend browsers (using DoH fallback).
- **Navigate to package**: `cd js`
- **Install**: `npm install email-intel`
- **Full Guide**: [Read the JS Documentation](./js/README.md)

### 🔵 Go
Extremely fast, concurrent execution using Go's native net resolver. Perfect for high-throughput APIs.
- **Navigate to package**: `cd go`
- **Install**: `go get github.com/itshivams/email-intel`
- **Full Guide**: [Read the Go Documentation](./go/README.md)

### 🐍 Python
Synchronous, robust DNS probing leveraging the powerful `dnspython` library. Great for Django/FastAPI.
- **Navigate to package**: `cd python`
- **Install**: `pip install email-intel`
- **Full Guide**: [Read the Python Documentation](./python/README.md)

---

## 🤝 Contributing

We welcome contributions across all three ecosystems! Because we maintain feature parity, logic changes in one language often require replicating the logic in the other two.

### To Start Contributing:
1. Fork the repository and clone it locally.
2. Navigate to the language package you'd like to work on (`/js`, `/go`, or `/python`).
3. Make your changes and write/update tests if applicable.
4. If modifying the core intelligence logic (e.g. `inferProvider` or `determineDomainType`), please implement those exact changes in **all three languages** to maintain parity!
5. Open a Pull Request.

---

## 🙏 Acknowledgements

A massive thank you to the open-source community for maintaining the massive datasets required to accurately flag domains. 

Specifically, this repository runs a daily automated cron job to fetch and normalize the latest domains from:
- [**disposable-email-domains/disposable-email-domains**](https://github.com/disposable-email-domains/disposable-email-domains) for their exhaustive list of temporary email providers.
- [**Kikobeats/free-email-domains**](https://github.com/Kikobeats/free-email-domains) for their comprehensive list of public webmail providers.

Without these incredibly well-maintained community lists, `email-intel` wouldn't be possible. Thank you!
