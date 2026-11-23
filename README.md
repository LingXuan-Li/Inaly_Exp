# Inaly Pulse monitored repository

このフォルダは監視対象リポジトリの雛形です。GitHub上に新しいリポジトリを作成し、このフォルダの中身をそのリポジトリのdefault branchへpushすると、ActionsからInaly Pulseを起動できます。

## 有効化

1. `config/monitors.json` の `target` を監視したい公開HTTP/DNS対象に変更します。初期設定では `https://example.com` と `example.com` を使います。
2. GitHub Appを作成し、`Metadata: read`、`Contents: read/write`、`Issues: read/write` を付け、このリポジトリだけにインストールします。
3. **Settings → Secrets and variables → Actions** で次を登録します。
   - Repository variable: `INALY_APP_ID` (App ID)
   - Repository secret: `INALY_APP_PRIVATE_KEY` (PEMファイル全体)
   - Repository secret: `DISCORD_WEBHOOK_URL` (Discord通知を使う場合)
   - Repository secret: `INALY_MONITOR_SECRETS` (認証付き監視を使う場合)
4. Actionsタブで `Inaly Pulse` が有効なことを確認し、`Run workflow` から最初の手動実行をします。

成功すると `data/state/`、`data/history/`、`data/incidents/`、`data/outbox/` のJSONがdefault branchに保存されます。Discord通知を使うには、`config/monitors.json` の `settings.notifications.discord.enabled` を `true` にしてください。

この雛形は `LingXuan-Li/inaly_pulse/.github/workflows/monitor.yml@main` をReusable Workflowとして呼び出します。開発リポジトリのOwnerまたはbranchを変えた場合は `.github/workflows/inaly-pulse.yml` の参照先も変更してください。

## メンテナンス

メンテナンス予定は `data/maintenance/schedules.json` にUTCで追加します。形式は開発リポジトリの `docs/maintenance-schedules.example.json` を参照してください。`suppress` はチェックを続けて外部反映を保留し、`skip` はチェック自体を省略します。
