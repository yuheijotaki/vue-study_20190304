# Vue.js / JSON から情報を引っ張ってくる その9

## やること

- WordPress で運用している [ポートフォリオサイト](https://works.yuheijotaki.com) と同様の機能を持ったサイトを Vue.js で実装する。
- 

#### HTML5 History モード

今回は [HTML5 History モード \| Vue Router](https://router.vuejs.org/ja/guide/essentials/history-mode.html#%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%AE%E8%A8%AD%E5%AE%9A%E4%BE%8B) を使用してみた。  
`/router/index.js` で `mode: 'history'` の指定を追加。  
参考：[Vue\.js \- vue\-routerの、hashモードと、historyモードの役割の違いについて｜teratail](https://teratail.com/questions/112717) 

```javascript
export default new Router({
  mode: 'history',
  routes: [
    {
      path: "/",
      name: 'top',
      component: top
    },
    {
      path: "/about",
      name: 'about',
      component: about
    }
  ]
})
```

#### `props` や `$emit`



## まとめ

カテゴリー選択した際のフィルターは、配列取得してfor文でマッチする投稿を出し分けしていますが、`filter` などを使えばもっとスマートに書けそう。





### 残り

- `router` の `/top/`  `/about/` 遷移時にナビをアクティブにする
- 投稿読み込み時ローディング