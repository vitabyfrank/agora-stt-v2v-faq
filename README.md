# Agora STT(V2V) — FAQ & Troubleshooting Guide

> **[한국어 README](./README.ko.md)**

A customer-facing FAQ and troubleshooting reference for the **Agora Speech-to-Speech Translation (V2V)** service.

## Overview

Single-page HTML document covering common questions, configuration pitfalls, and error resolution for the Agora STT V2V API. Supports Korean / English toggle.

## Sections

| Section | Description |
|---------|-------------|
| Getting Started | Service activation via Agora Console |
| Usage | How to check STT(V2V) usage |
| FAQ | Multi-language, multi-agent, UID/token config, 409 conflict, 200-but-fail |
| Troubleshooting | Structured template for reporting issues |
| Error Codes | 200, 401, 403, 404, 409, 500 with recommended actions |
| Resources | Demo and official documentation links |

## Features

- **Bilingual toggle** — Korean ↔ English
- **Sidebar navigation** — Scroll-aware active state, click-to-center UX
- **Highlight on navigate** — Visual highlight box on clicked items
- **Responsive** — Mobile-friendly with collapsible sidebar
- **Print-ready** — Sidebar and toggle hidden in print layout
- **Zero dependencies** — Pure HTML/CSS/JS, no build step required

## Quick Start

Open `agora-qna.html` in any browser, or serve via a static file server:

```bash
# Python
python3 -m http.server 5500

# Node.js
npx serve .
```

## Resources

- [STT(V2V) Demo](https://dl.agoralab.co/v2v/)
- [STT(V2V) Documentation](https://dl.agoralab.co/v2v/docs/#/ko/)
- [Agora Console](https://console.agora.io/)
