---
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
marp: true

---
![bg left:45% 100%](https://cf-assets.www.cloudflare.com/slt3lc6tev37/5WxOgxmuzimF1eDXUt8rGB/fa23a45aa8c709cc04dfa93076ef1cf8/BDES-5042_-_Collaboration-Team-Chat-Approval.svg)

# DurableObjects
# 完全に理解した

---

# CloudflareWorkers(CW)とは

**エッジ**で動作する**軽量**で**サーバーレス**な**Javascript実行環境**

- CloudflareのCDNエッジで動作（**Like**: Vercel Edge Functions, CloudFront Functions/**NotLike**: AWS Lambda, Cloud Run関数）

- 0ms cold starts
  - V8エンジン
  - isolated

- **ステートレス**

---

# CloudflareWorkersとは

Workersにはエッジでサーバーレスアプリケーションを構築するためのエコシステムが充実している

- KSV: Workers KV
- オブジェクトストレージ: R2
- RDBMS: D1
- **Durable Objects** <- こいつについて

---

# Durable Objects(DO)とは

CloudflareWorkersから参照可能な**インメモリの永続する共通状態**

- どのWorkersからアクセスしても一貫性が保たれる

- 結果整合ではなく強整合なのでトランザクション的に扱える

- CDNエッジで動作するので低レイテンシ（と言うか裏側はWorkersでできている）

---

# DurableObjects(DO)とは

<style scoped>
p { text-align: center; }
section {
    font-size: 30px;
}
</style>

![width:1040px center](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5HLQ06eTIRyAl30bPTlVov/975cd2e8aa2360c4eab7e4c5a6d440ef/image1-65.png)

ref: [Durable Objects, now in Open Beta](https://blog.cloudflare.com/durable-objects-open-beta/)

---

# 何が嬉しいのか

ステートレスなサーバーレス環境（Workers）が永続するメモリ（状態）を扱える！

**-> WebSocketを扱える！**
  - チャット
  - 共同編集機能
  - オンラインゲーム

これらはKVやRDBMSで実現するには無理がある

---
### デモ: 永続カウンター

DurableObjectsはコード上でクラスとして宣言する（オブジェクト指向プログラミングにおける**オブジェクト**）

```javascript
// どのClassがDOかはwrangler.tomlに記載
export class Counter {
  constructor(controller, env) {
     // インスタンスの初期化メソッド
  }
  // 中略
  // HTTPリクエストハンドラー
  async fetch(request) {
     // 中略
    return new Response(this.value);
  }
}
```

---
### デモ: 永続カウンター

全コードは[こちら](https://github.com/cloudflare/durable-objects-template)

デモサイト: https://worker.ikefukurou12.workers.dev 

カウント++: https://worker.ikefukurou12.workers.dev/increment

カウント--: https://worker.ikefukurou12.workers.dev/decrement

![width:200px](demo.png)

---

### refs

<style scoped>
section {
    font-size: 27px;
}
</style>

公式ブログ（これだけ読んでおけばいい）

- [Workers Durable Objects Beta: A New Approach to Stateful Serverless](https://blog.cloudflare.com/introducing-workers-durable-objects/)
- [Eliminating cold starts with Cloudflare Workers](https://blog.cloudflare.com/eliminating-cold-starts-with-cloudflare-workers/)


いい感じの記事・ブログ
- [CDN 入門とエッジでのアプリケーション実行](https://future-architect.github.io/articles/20230427a/)
- [Cloudflare WorkersのDurable Objectsでチャットを実装する](https://leaysgur.github.io/posts/2021/03/29/140743/)
- [Cloudflare Durable Object 入門](https://zenn.dev/kameoncloud/articles/abffdea40ffa50)

スライド作り
- [Marpit: Markdown slide deck framework](https://marpit.marp.app/)
- [Marp(Markdown Presentaiton)の文法まとめ](https://qiita.com/pocket8137/items/27ede821e59c12a1b222#-%E3%83%87%E3%82%A3%E3%83%AC%E3%82%AF%E3%83%86%E3%82%A3%E3%83%96%E3%82%92%E7%8F%BE%E5%9C%A8%E3%81%AE%E3%82%B9%E3%83%A9%E3%82%A4%E3%83%89%E3%81%AB%E3%81%AE%E3%81%BF%E9%81%A9%E7%94%A8%E3%81%99%E3%82%8B)