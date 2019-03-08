# Vue.js / JSON から情報を引っ張ってくる その9

## やること

- WordPress で運用している [ポートフォリオサイト](https://works.yuheijotaki.com) と同様の機能を持ったサイトを Vue.js で実装する。
- Vue Router を使用して History モードでページ遷移を行う。

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

#### vue-router 使用時の現在地ナビ（カレント表示）

現在いるページのナビゲーション `<a>` 要素にはデフォルトで `.router-link-exact-active` というクラスが付与されるが、それを変更したい場合

```html
<ul>
  <li><router-link to="/" exact-active-class="is-selected">Top</router-link></li>
  <li><router-link to="/about" exact-active-class="is-selected">About</router-link></li>
</ul>
```

のように `exact-active-class="is-selected"` としてあげる

参考：[API Reference \| Vue Router](https://router.vuejs.org/api/#event)  
参考：[Vuejs vue\-routerはアクティブなリンクに自動でクラスを振ってくれる \- Qiita](https://qiita.com/kimullaa/items/a75a47f504c75058081f)

## まとめ

[**Github**](https://github.com/yuheijotaki/vue-study_20190304)

- カテゴリー選択した際のフィルターは、配列取得してfor文でマッチする投稿を出し分けしていますが、`filter` などを使えばもっとスマートに書けそう。

- 各投稿をカテゴリーごとに出力する箇所、結局 `$emit` や `props` を使わずに カテゴリー一覧と投稿出力のコンポーネントを分けるのは断念した。そもそもできるものなのかも不明ですが、このあたりも自由にできたら楽しいだろうなと思う。
- やっとサイトっぽくなったのはよかったかなと思います。