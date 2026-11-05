# message-corpus

A public corpus of redacted spam, scam, and phishing messages (emails and text messages) for security research, awareness training, and classifier development.

All samples are real-world messages. Contributor PII has been redacted and URLs have been defanged before publication.

---

## Intended usage

This corpus is intended for:

- **Security awareness teams** — realistic phishing and smishing samples for
  training campaigns, without handling live malicious content
- **Researchers** — studying how threat actors craft messages: subject lines,
  sender spoofing, urgency tactics, HTML structure, lure techniques
- **Developers** — building and evaluating spam/phishing classifiers with
  real-world samples
- **Educators** — illustrating phishing techniques in a safe, controlled format

This corpus is **not** intended as a live threat intelligence feed. URLs are
defanged and, for emails, routing headers are stripped — the samples are for
analysis, not for direct import into blocking tools.

Samples are not sub-classified. Every item is unsolicited and unwanted, but
whether it is spam, a scam, or phishing is left to the user.

---

## What's in here

### Emails

Each sample is a redacted `.eml` file. Per sample:

- Routing headers (`Received:`, `Return-Path:`, etc.) are stripped
- Contributor email address and name variants are replaced with
  `redacted@redacted.com` / `redacted`
- URLs in body text are defanged (`hxxps://`, `[.]` notation)
- Email structure, HTML lure, and attachments are otherwise preserved

### Text messages

Each sample is a `.json` file, one per message, with three fields:

- `timestamp` — time received, UTC, ISO 8601
- `sender` — the number or address the message came from, **as received**
- `body` — the message text, with URLs defanged (`hxxps://`, `[.]` notation)

Text messages are inbound only, text content only (no attachments), and were
reviewed manually for contributor identifiers before publication. The
contributor's own number is not included anywhere in the data.

---

## Structure

```
submissions/
  email/
    <addr-hash>/          <- opaque contributor ID (hash)
      <version>/          <- tool version that produced these outputs
        <sha256>.eml      <- redacted sample
  <service>/              <- message service, e.g. imessage
    <recipient-hash>/     <- opaque contributor ID (keyed hash), no PII
      <version>/          <- extractor version that produced these outputs
        <guid>.json       <- sample
```

Future messaging sources (e.g. Signal, WhatsApp) will get their own sibling
folder under `submissions/`.

---

## Disclaimer

These samples are provided **as-is** for research and educational purposes
only. No warranty is made as to completeness, accuracy, or fitness for any
particular purpose.

- **Redaction is best-effort.** Despite automated redaction and manual
  verification, residual PII may remain. Do not use these samples to identify
  or contact any individual.
- **Sender identifiers are preserved in text messages.** Sender numbers and
  addresses are kept because they are useful for research, but they may be
  spoofed or may belong to innocent third parties. Do not treat a sender
  number as proof of who sent a message, and do not contact it.
- **Defanging is not sanitisation.** URLs are defanged for display safety but
  the underlying domains and paths are preserved. Do not re-fang and visit URLs
  without appropriate precautions.
- **Attachments are preserved in emails.** Some samples may contain attachments
  (e.g. calendar invites). No executables, Office documents, or PDFs are
  knowingly included, but exercise caution when opening samples.
- **Use responsibly.** Do not use these samples to train models intended for
  malicious purposes, to harass individuals, or for any unlawful activity.

---

## Reporting issues

If you find any of the following, please open a GitHub issue:

- **Residual PII** — contributor name, email address, phone number, or other
  personal information that was not redacted
- **Malicious attachments** — executables, Office documents, PDFs, or other
  potentially harmful file types
- **Other concerns** — anything that should not be in a public corpus

---

## Tools

- Emails are processed using
  [eml-contrib-ng](https://github.com/cw-l/eml-contrib-ng).
- Text messages are extracted from the contributor's Apple Messages database.