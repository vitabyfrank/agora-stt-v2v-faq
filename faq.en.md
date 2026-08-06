# Agora STT(V2V) FAQ and Troubleshooting

Last updated: 2026-07-16

## Getting Started

### How do I get started with the STT(V2V) service?

You must first enable **Conversational AI Engine** in the Agora Console. Without activation, API calls will return permission errors.

How to enable: Agora Console -> Select Project -> Conversational AI Engine -> Enable

## Supported Languages

Use these language codes in `source_language` and `target_language` settings. The same list is searchable in the HTML page.

| Language | Code |
| --- | --- |
| Korean (Korea) | `ko-KR` |
| English (United States) | `en-US` |
| Vietnamese (Vietnam) | `vi-VN` |
| Chinese | `zh-CN` |
| Thai (Thailand) | `th-TH` |
| Japanese (Japan) | `ja-JP` |
| Spanish (Spain) | `es-ES` |
| French (France) | `fr-FR` |
| Filipino (Philippines) | `tl-PH` |
| Russian (Russia) | `ru-RU` |
| Afrikaans (South Africa) | `af-ZA` |
| Arabic | `ar-AE` |
| Norwegian | `nb-NO` |
| Catalan | `ca-ES` |
| Azerbaijani (Latin, Azerbaijan) | `az-AZ` |
| Belarusian | `be-BY` |
| Bulgarian (Bulgaria) | `bg-BG` |
| Bengali (India) | `bn-IN` |
| Bosnian (Bosnia and Herzegovina) | `bs-BA` |
| Czech (Czechia) | `cs-CZ` |
| Welsh (United Kingdom) | `cy-GB` |
| Danish (Denmark) | `da-DK` |
| German (Germany) | `de-DE` |
| Greek (Greece) | `el-GR` |
| Estonian (Estonia) | `et-EE` |
| Basque | `eu-ES` |
| Persian (Iran) | `fa-IR` |
| Finnish (Finland) | `fi-FI` |
| Galician | `gl-ES` |
| Gujarati (India) | `gu-IN` |
| Hebrew (Israel) | `he-IL` |
| Hindi (India) | `hi-IN` |
| Croatian (Croatia) | `hr-HR` |
| Hungarian (Hungary) | `hu-HU` |
| Indonesian (Indonesia) | `id-ID` |
| Italian (Italy) | `it-IT` |
| Kazakh (Kazakhstan) | `kk-KZ` |
| Kannada (India) | `kn-IN` |
| Lithuanian (Lithuania) | `lt-LT` |
| Latvian (Latvia) | `lv-LV` |
| Macedonian (North Macedonia) | `mk-MK` |
| Malayalam (India) | `ml-IN` |
| Marathi (India) | `mr-IN` |
| Malay (Malaysia) | `ms-MY` |
| Dutch (Netherlands) | `nl-NL` |
| Punjabi (India) | `pa-IN` |
| Polish (Poland) | `pl-PL` |
| Portuguese | `pt-PT` |
| Romanian (Romania) | `ro-RO` |
| Slovak (Slovakia) | `sk-SK` |
| Slovenian (Slovenia) | `sl-SI` |
| Albanian (Albania) | `sq-AL` |
| Serbian (Cyrillic, Serbia) | `sr-RS` |
| Swedish (Sweden) | `sv-SE` |
| Kiswahili (Kenya) | `sw-KE` |
| Kiswahili (Tanzania) | `sw-TZ` |
| Tamil (India) | `ta-IN` |
| Telugu (India) | `te-IN` |
| Turkish | `tr-TR` |
| Ukrainian | `uk-UA` |
| Urdu (India) | `ur-IN` |

## Frequently Asked Questions

### Does this API support multiple languages?

Yes, but you need to start a separate agent for each language.

### Can I run multiple agents in a single channel at the same time?

Yes. Assign a unique agent name to each agent and they can operate in parallel within the same channel.

### The agent is not responding during a call.

Please fill out the troubleshooting template below and share it with support.

### API returns 200 (success), but the agent is not working.

The RTC token server and Agent server operate independently. Even if the STT server accepts the request and returns a 200 response, the agent will not function properly if there is an issue with the RTC token.

Check: Verify that the token was generated for the correct `agent_rtc_uid` and that it has not expired.

### Can a single worker translate multiple languages at the same time?

No. The STT(V2V) service is designed to support **only one translation language per worker** for quality and stability. For multi-language subtitles, configure separate workers for each target language.

Example:

```text
"name": "translation_en" -> Korean -> English
"name": "translation_zh" -> Korean -> Chinese
```

### What is the difference between `agent_rtc_uid` and `remote_rtc_uids`?

`agent_rtc_uid` is the agent's translation bot UID. It specifies the unique ID for the agent joining the channel.

`remote_rtc_uids` is the list of target user UIDs. It specifies the actual users whose audio the agent will receive and translate.

Common mistake: Using a non-existent UID or the agent's own UID in `remote_rtc_uids`. Always use the UID of a user who is active in the channel.

### How should I generate the agent token?

The `token` must be generated for the `agent_rtc_uid`.

Example: if `agent_rtc_uid` is `"1002"`, generate the token for UID 1002.

Note: Using a user's token in place of the agent's token will result in an error.

### Starting multiple translation agents in the same channel returns a TaskConflict error.

Each agent must have a unique `name` field. Starting a second agent with the same `name` causes a conflict with the existing agent, resulting in a `409 TaskConflict` error.

Solution: Assign a unique name per language.

```text
"name": "agora_translation_test_id_en" -> English Agent
"name": "agora_translation_test_id_zh" -> Chinese Agent
```

## Troubleshooting Template

When facing issues, please provide the following information for faster resolution.

### Basic Info

- Channel Name: e.g. `test_channel_01`
- Agent ID: e.g. `agent-abc123`
- App ID: e.g. `aab236bhelshyg27`
- Timestamp: e.g. `2026-03-17 14:30 KST`

### RESTful API Info

- API Endpoint: e.g. `POST /v1/projects/{appid}/join`
- Request Body: JSON body
- Response Status Code: e.g. `401`, `500`, `200`
- Response Body: Paste the full error message

### Additional Context

- Environment: e.g. Web SDK v4.23.0
- Description: e.g. Agent joins the channel and receives audio input, but STT results are not delivered to the client
- Error Message: e.g. `{"code": "AGENT_TIMEOUT", "message": "Agent failed to respond within 10s"}`
- Steps to Reproduce:
  1. Create a project with App ID `aab236b***` in Console
  2. Call POST `/v1/projects/{appid}/join` to join the agent to channel `test_channel_01`
  3. Join the same channel from client (Web SDK v4.23.0)
  4. Client sends voice; agent performs STT for about 5 seconds, then no response
  5. Check the error
- What I've Tried: e.g. Regenerated token, tested on a different channel, changed `agent_rtc_uid`, etc.
- Key Finding: e.g. Same config works with a different App ID; issue is specific to this App ID only
- Relevant Logs: Paste console logs, network request/response, SDK logs, etc.

Attach logs or screenshots if available. The more detail you provide, the faster the issue can be resolved.

## Common Error Codes

| Code | Meaning | Action |
| --- | --- | --- |
| 200 | Success (Caution) | The agent may not work even with 200 if the token is invalid. Verify the token was generated for the correct `agent_rtc_uid` and has not expired. |
| 401 | Unauthorized | Verify Token / Client ID / Client Secret. |
| 403 | Forbidden | Check project permissions and IP allowlist. |
| 409 | Task Conflict | Use a unique agent `name`. |
| 404 | Not Found | Verify Endpoint URL, App ID, and RESTful API Parameters. |
| 500 | Internal Server Error | Contact support with the troubleshooting template above. |

## Resources

- [STT(V2V) Demo](https://dl.agoralab.co/v2v/)
- [STT(V2V) Documentation](https://dl.agoralab.co/v2v/docs/#/ko/)
- [Agora Console](https://console.agora.io/)
