# Agora STT(V2V) FAQ 与故障排查

最后更新：2026-07-16

## 开始使用

### 如何开始使用 STT(V2V) 服务？

你需要先在 Agora Console 中启用 **Conversational AI Engine**。如果未启用，调用 API 时会返回权限错误。

启用方式：Agora Console -> 选择项目 -> Conversational AI Engine -> 启用(Enable)

## 支持的语言

语言代码用于 `source_language` 和 `target_language` 配置。同一列表也可在 HTML 页面中搜索。

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

## 常见问题

### 该 API 是否支持多种语言？

支持。但需要为每种语言分别启动一个独立的智能体。

### 能否在同一个频道中同时运行多个智能体？

可以。为每个智能体的 `name` 字段设置唯一值后，它们可以在同一频道内并行运行。

### 通话过程中智能体没有响应。

请填写下方故障排查模板并提供给支持团队。

### API 返回 200(成功)，但智能体没有正常工作。

RTC Token 服务器和 Agent 服务器是独立运行的。即使 STT 服务器正常接收请求并返回 200，如果 RTC Token 存在问题，智能体也无法加入频道。

检查项：请确认 Token 是否按正确的 `agent_rtc_uid` 生成，以及 Token 是否仍在有效期内。

### 一个 worker 能否同时翻译多种语言？

不能。为保证质量和稳定性，STT(V2V) 服务设计为**每个 worker 仅支持一种目标翻译语言**。如果需要多语言字幕，请按目标语言分别配置不同的 worker。

示例：

```text
"name": "translation_en" -> 韩语 -> 英语
"name": "translation_zh" -> 韩语 -> 中文
```

### `agent_rtc_uid` 和 `remote_rtc_uids` 有什么区别？

`agent_rtc_uid` 是智能体(翻译机器人)的 UID。用于指定要加入频道的智能体唯一 ID。

`remote_rtc_uids` 是目标用户 UID 列表。用于指定智能体需要接收并翻译其音频的真实用户。

常见错误：在 `remote_rtc_uids` 中填写不存在的 UID，或填写智能体自己的 UID。请务必填写当前已加入频道的用户 UID。

### 智能体 Token 应该如何生成？

`token` 必须为 `agent_rtc_uid` 对应的智能体 UID 生成。

例如：`agent_rtc_uid` 为 `"1002"` 时，生成 UID 1002 的 Token。

注意：如果把用户 Token 填到智能体 Token 的位置，会导致错误。

### 在同一频道中启动多个翻译智能体时返回错误。

每个智能体的 `name` 字段都必须使用唯一值。如果用相同的 `name` 启动第二个智能体，会与已有智能体冲突，并返回 `409 TaskConflict` 错误。

解决方法：为每种语言设置唯一的 name。

```text
"name": "agora_translation_test_id_en" -> 英语智能体
"name": "agora_translation_test_id_zh" -> 中文智能体
```

## 故障排查模板

遇到问题时，请一并提供以下信息，以便更快定位和解决。

### 基本信息

- 频道名称：例如 `test_channel_01`
- 智能体 ID：例如 `agent-abc123`
- App ID：例如 `aab236bhelshyg27`
- 发生时间：例如 `2026-03-17 14:30 KST`

### RESTful API 信息

- API 端点：例如 `POST /v1/projects/{appid}/join`
- 请求体：JSON 请求体
- 响应状态码：例如 `401`, `500`, `200`
- 响应体：粘贴完整错误信息

### 补充信息

- 环境：例如 Web SDK v4.23.0
- 问题描述：例如智能体加入频道并接收到音频输入，但 STT 转写结果没有发送到客户端
- 错误信息：例如 `{"code": "AGENT_TIMEOUT", "message": "Agent failed to respond within 10s"}`
- 复现步骤：
  1. 在 Console 中使用 App ID `aab236b***` 创建项目
  2. 调用 POST `/v1/projects/{appid}/join`，让智能体加入 `test_channel_01` 频道
  3. 客户端(Web SDK v4.23.0)加入同一频道
  4. 客户端发送语音后，智能体执行约 5 秒 STT 转写，随后无响应
  5. 查看错误
- 已尝试的解决方法：例如重新生成 Token、在其他频道测试、修改 `agent_rtc_uid` 等
- 关键发现：例如相同配置在其他 App ID 下正常，仅特定 App ID 出现问题
- 相关日志：请粘贴控制台日志、网络请求/响应、SDK 日志等信息

如有日志文件或截图，请一并附上。信息越详细，问题解决越快。

## 常见错误码

| Code | 含义 | 处理方式 |
| --- | --- | --- |
| 200 | 成功(注意) | 即使返回 200，如果 Token 无效，智能体也可能无法工作。请确认 Token 是否按正确的 `agent_rtc_uid` 生成且仍在有效期内。 |
| 401 | 认证失败 | 重新检查 Token / Client ID / Client Secret。 |
| 403 | 无权限 | 检查项目权限和 IP 白名单。 |
| 409 | 任务冲突 | 为智能体设置唯一的 `name` 字段值。 |
| 404 | 资源不存在 | 检查 Endpoint URL、App ID 和 RESTful API 参数。 |
| 500 | 服务器内部错误 | 使用上方模板联系支持团队。 |

## 资源

- [STT(V2V) 演示](https://dl.agoralab.co/v2v/)
- [STT(V2V) 官方文档](https://dl.agoralab.co/v2v/docs/#/ko/)
- [Agora Console](https://console.agora.io/)
