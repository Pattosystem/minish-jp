# MINISH Japan Partner — Landing Page

韓国・江南の [MINISH歯科病院](https://minishtech.com/ja/) と日本のお客様を繋ぐ、日本語対応の紹介・渡韓サポート用ランディングページ。

---

## URL

本番公開URL（GitHub Pages）:

```
https://pattosystem.github.io/minish-jp/
```

---

## QRコード用URLパターン

各パートナークリニックに配布するQRコードは、以下のパラメータを含めます：

```
https://pattosystem.github.io/minish-jp/?ref=CLINIC001&src=leaflet&v=1
```

| パラメータ | 説明 | 例 |
|---|---|---|
| `ref` | クリニックID（必須） | `CLINIC001` / `OSAKA001` |
| `src` | 媒体種別 | `leaflet` / `poster` / `verbal` |
| `v` | 印刷物バージョン | `1` / `2025-Q1` |
| `debug` | デバッグバッジ表示（開発用） | `1` |

### テスト用URL

```
# 紹介元バナー表示あり
?ref=CLINIC001&src=leaflet&v=1

# 未登録クリニックのフォールバック確認
?ref=UNKNOWN999

# デバッグバッジ表示
?ref=CLINIC001&debug=1

# 紹介元なしアクセス（ポップアップが出る）
（パラメータなし）
```

---

## 登録済みクリニック

現在 `index.html` 内のJavaScript `CLINICS` オブジェクトで管理中：

| ID | 表示名 | エリア |
|---|---|---|
| `CLINIC001` | 銀座ビューティークリニック | Tokyo · Ginza |
| `CLINIC002` | 表参道スキンクリニック | Tokyo · Omotesando |
| `OSAKA001` | 心斎橋メディカルビューティー | Osaka · Shinsaibashi |
| `VANDS001` | Vands Clinic 大阪院 | Osaka · Umeda |

新規クリニック追加は、`index.html` の `const CLINICS = {...}` を編集してください。将来的にはSupabase/GASから動的取得に移行予定。

---

## トラッキングの仕組み

1. URLの `?ref=` がある → sessionStorage に保存 → 各種バナー＆フォームhidden fieldに反映
2. `?ref=` がない初回訪問 → ポップアップで自己申告可
3. ポップアップをスキップ → フォーム内に紹介元フィールドがフォールバック表示
4. フォーム送信時、バックエンドに以下を送信：
   - `referrer_id` / `referrer_source` / `referrer_version`
   - `landed_at` / `submitted_at`
   - `referrer_self_reported` / `referrer_self_custom_name`（自己申告の場合）

---

## 開発・メンテナンス

### ローカルで確認

```bash
# リポジトリルートで
python3 -m http.server 8000
# → http://localhost:8000/
```

### 画像アセットについて

現状、以下のMINISH公式CDNを**ホットリンク**しています（プロトタイプ段階）：
- ロゴ: `https://minishtech.com/wp-content/uploads/2025/10/Logo.svg`
- 商品画像: `https://minishtech.com/wp-content/uploads/2025/04/prod_img_0N.png`

**本番運用前に必ず**：
1. MINISH本部から正式な素材使用許諾を取得
2. 画像を本リポジトリの `images/` にダウンロード
3. `index.html` のURLをローカルパス（`./images/...`）に書き換え

### Before/After症例写真

MINISH本部から提供された写真を `images/cases/` に配置し、`index.html` 内の該当箇所を差し替え：

```html
<!-- Before -->
<div class="ba-slot" data-label="Before"
     style="background-image:url('./images/cases/case01-before.jpg')"></div>
<!-- After -->
<div class="ba-slot" data-label="After"
     style="background-image:url('./images/cases/case01-after.jpg')"></div>
```

`placeholder` クラスと `data-placeholder` 属性を削除。

---

## TODO

- [ ] MINISH本部と提携正式化 · 素材使用許諾取得
- [ ] 料金レンジ確定（現在「調整中」表記）
- [ ] Before/After写真の差し替え
- [ ] バックエンド接続（GAS Web App or Supabase）
- [ ] QRコード発行管理画面の構築
- [ ] 既存の紹介コミッションシステムとの統合
- [ ] 画像アセットをローカルホストに移行

---

## License

© 2025 MINISH Japan Partner. All rights reserved.
MINISH® is a registered trademark of MINISH Technology Inc. (Republic of Korea).
