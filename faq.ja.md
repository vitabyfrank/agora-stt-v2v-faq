# Agora STT(V2V) FAQとトラブルシューティング

最終更新: 2026-07-16

## はじめに

### STT(V2V)サービスを利用するにはどうすればよいですか？

まず Agora Console で **Conversational AI Engine** を有効化する必要があります。有効化していない場合、API呼び出し時に権限エラーが返されます。

有効化手順: Agora Console -> プロジェクトを選択 -> Conversational AI Engine -> 有効化(Enable)

## 対応言語

言語コードは `source_language` と `target_language` の設定で使用します。同じ一覧はHTMLページでも検索できます。

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

## よくある質問

### このAPIは複数の言語に対応していますか？

はい、対応しています。ただし、言語ごとに個別のエージェントを起動する必要があります。

### 1つのチャンネルで複数のエージェントを同時に利用できますか？

可能です。各エージェントに一意の agent name を付与すると、同じチャンネル内で並列に動作します。

### 通話中にエージェントが応答しません。

下記のトラブルシューティングテンプレートに記入し、サポートチームへ共有してください。

### APIレスポンスは200(成功)ですが、エージェントが動作しません。

RTCトークンサーバーと Agent サーバーは独立して動作します。STTサーバーがリクエストを正常に受け付けて200レスポンスを返しても、RTCトークンに問題がある場合、エージェントは正常に動作しません。

確認事項: トークンが正しい `agent_rtc_uid` に対して発行されているか、有効期限が切れていないかを確認してください。

### 1つの worker で複数の言語を同時に翻訳できますか？

いいえ。STT(V2V)サービスは品質と安定性のため、**worker 1つにつき翻訳先言語は1つ**のみ対応するように設計されています。多言語字幕が必要な場合は、言語ごとに worker を分けてそれぞれ設定してください。

例:

```text
"name": "translation_en" -> 韓国語 -> 英語
"name": "translation_zh" -> 韓国語 -> 中国語
```

### `agent_rtc_uid` と `remote_rtc_uids` の違いは何ですか？

`agent_rtc_uid` はエージェント(翻訳ボット)のUIDです。チャンネルに参加するエージェントの一意のIDを指定します。

`remote_rtc_uids` は翻訳対象ユーザーのUID一覧です。エージェントが音声を受信して翻訳する実際のユーザーを指定します。

よくあるミス: `remote_rtc_uids` に存在しないUIDやエージェント自身のUIDを指定してしまうケースがあります。必ずチャンネルに参加中のユーザーUIDを指定してください。

### エージェントトークンはどのように発行すればよいですか？

`token` は必ず `agent_rtc_uid` に対応するエージェント用のトークンとして発行する必要があります。

例: `agent_rtc_uid` が `"1002"` の場合、UID 1002 用のトークンを生成します。

注意: ユーザーのトークンをエージェントトークンとして使用するとエラーになります。

### 同じチャンネルで複数の翻訳エージェントを開始するとエラーが発生します。

各エージェントの `name` フィールドには一意の値を使用する必要があります。同じ `name` で2つ目のエージェントを開始すると、既存のエージェントと競合し、`409 TaskConflict` エラーが発生します。

解決方法: 言語ごとに一意の name を設定してください。

```text
"name": "agora_translation_test_id_en" -> 英語エージェント
"name": "agora_translation_test_id_zh" -> 中国語エージェント
```

## トラブルシューティングテンプレート

問題が発生した場合は、迅速な解決のために以下の情報を共有してください。

### 基本情報

- チャンネル名: 例: `test_channel_01`
- エージェントID: 例: `agent-abc123`
- App ID: 例: `aab236bhelshyg27`
- 発生時刻: 例: `2026-03-17 14:30 KST`

### RESTful API情報

- API エンドポイント: 例: `POST /v1/projects/{appid}/join`
- リクエスト本文: JSON ボディ
- レスポンスステータスコード: 例: `401`, `500`, `200`
- レスポンス本文: エラーメッセージ全文を貼り付けてください

### 追加情報

- 環境: 例: Web SDK v4.23.0
- 問題の説明: 例: エージェントはチャンネルに参加して音声入力を受信するが、STT変換結果がクライアントに配信されない
- エラーメッセージ: 例: `{"code": "AGENT_TIMEOUT", "message": "Agent failed to respond within 10s"}`
- 再現手順:
  1. Consoleで App ID `aab236b***` のプロジェクトを作成
  2. POST `/v1/projects/{appid}/join` を呼び出し、エージェントをチャンネル `test_channel_01` に参加させる
  3. クライアント(Web SDK v4.23.0)から同じチャンネルに参加
  4. クライアントが音声を送信すると、エージェントが約5秒間STT変換を実行した後、応答しない
  5. エラーを確認
- 試した解決方法: 例: トークンの再発行、別チャンネルでのテスト、`agent_rtc_uid` の変更など
- 主な発見事項: 例: 同じ設定で別の App ID では正常に動作し、特定の App ID でのみ問題が発生する
- 関連ログ: コンソールログ、ネットワークリクエスト/レスポンス、SDKログなどを貼り付けてください

ログファイルやスクリーンショットがあれば添付してください。情報が詳細であるほど、より早く問題を解決できます。

## 主なエラーコード

| Code | 意味 | 対応 |
| --- | --- | --- |
| 200 | 成功(注意) | 200でもトークンが無効な場合、エージェントが動作しないことがあります。トークンが正しい `agent_rtc_uid` に対して発行され、有効期限が切れていないか確認してください。 |
| 401 | 認証失敗 | Token / Client ID / Client Secret を再確認してください。 |
| 403 | 権限なし | プロジェクト権限とIP許可リストを確認してください。 |
| 409 | タスク競合 | エージェントの `name` を一意に設定してください。 |
| 404 | リソースが見つかりません | エンドポイント URL、App ID、RESTful API パラメーターを確認してください。 |
| 500 | 内部サーバーエラー | 上記テンプレートを使用してサポートにお問い合わせください。 |

## リソース

- [STT(V2V)デモ](https://dl.agoralab.co/v2v/)
- [STT(V2V)公式ドキュメント](https://dl.agoralab.co/v2v/docs/#/ko/)
- [Agora Console](https://console.agora.io/)
