# portfolio-dashboard

社内向けシステム鳥瞰図・ダッシュボードプラットフォームのリポジトリです。

## 目的

**本リポジトリは個人の学習目的で作成したものです。**

ITポートフォリオの可視化や、システム構成・プロジェクトの一覧表示など、社内で利用するダッシュボードの試作・練習を行っています。

## フォルダ構成

```
portfolio-dashboard/
├── index.html          # トップページ
├── pages/              # 各ビューのHTML（ITポートフォリオ・プロジェクト・アプリケーション層）
├── js/                 # ページ用スクリプト
├── config/             # 設定（config.js, config.yaml, i18n.js）
├── data/               # 表示用JSONデータ
├── assets/css/         # Tailwind 入力 (input.css) とビルド出力 (output.css)
├── docs/               # ドキュメント（依存関係など）
├── tailwind.config.js
└── package.json
```

※ ソース間の依存関係は [docs/DEPENDENCIES.md](docs/DEPENDENCIES.md) を参照してください。

## 免責事項

- **表示内容の正確性**: 本リポジトリで公開している表示内容は、設定データ等に基づいて作成したものであり、その正確性・完全性については一切保証いたしません。
- **利用に関する責任**: 本リポジトリで公開している情報（ダッシュボードの表示・解釈など）を利用したことにより生じた、いかなる結果・損害についても、作者は責任を負いかねます。業務上の判断は、ご自身の責任で行ってください。

## 静的ファイル（ダッシュボード・ビュー）

GitHub Pages で公開している場合は、[こちら](https://hiroyasu-komaki.github.io/portfolio-dashboard/)から次のページへ遷移できます。

| ファイル | 内容 |
|---------|------|
| `index.html` | トップ（ダッシュボード入口） |
| `pages/it_portfolio_dashboard.html` | ITポートフォリオダッシュボード |
| `pages/project_view.html` | プロジェクトビュー |
| `pages/application_layer_view.html` | アプリケーション層ビュー |

## ライセンス

[MIT License](LICENSE) の下で公開しています。
