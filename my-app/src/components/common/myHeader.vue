<template>
    <header>
      <nav>
        <ul>
          <li><router-link to="/">Top</router-link></li>
          <li><router-link to="/about">About</router-link></li>
          <li v-for="(category,index) in categories" :key="index">
            <a href="javascript:void(0);" @click="filterCategory" :data-category="category.name" :class="['naviLink' , { 'is-selected': category.selected }]">{{category.name}}</a>
          </li>
        </ul>
      </nav>
    </header>
</template>

<script>
import axios from "axios";

export default {
  name: 'myHeader',
  data() {
    return {
      categories: [
        {
          name: 'All',
          selected: true
        },
        {
          name: 'Front-end',
          selected: false
        },
        {
          name: 'WordPress',
          selected: false
        },
        {
          name: 'Web Design',
          selected: false
        },
        {
          name: 'Tumblr',
          selected: false
        }
      ],
      posts: []
    }
  },
  props: ['display'],
  methods: {
    request: function(){

    },
    filterCategory: function(event) { // カテゴリーがクリックされたとき用のメソッド
      // 全体のナビゲーションのクラス削除
      var targetElements = document.getElementsByClassName('naviLink');
      [].forEach.call(targetElements, function(elem) {
        elem.classList.remove('is-selected');
      });
      // 選択したナビゲーションのクラス付与
      event.currentTarget.classList.add('is-selected');

      // 投稿の取得
      axios.get( 'https://works.yuheijotaki.com/wp-json/wp/v2/posts?per_page=100' )
      .then( response => {
        this.posts = response.data;
        // console.log(this.posts);
      })
      .catch( error => {
        console.log(error);
      });

      const posts = this.posts;
      // console.log(posts);

      const selectedCategory = event.currentTarget.getAttribute('data-category'); // クリックしたカテゴリーの取得
      if ( selectedCategory !== 'All' ) {
        // `All` 以外を選択した場合
        for (var i = 0; i < posts.length; i++) { // 投稿ごとのループ
          const categories = posts[i].category_name; // 投稿のカテゴリーを取得
          const categoriesArray = categories.split(' ,'); // 取得したカテゴリーを配列に変換
          for (var j = 0; j < categoriesArray.length; j++) { // 投稿内のカテゴリーごとのループ
            if ( categoriesArray.indexOf(selectedCategory) >= 0 ) { // 投稿に属するカテゴリーが含まれる場合
              posts[i].customData.display = true;
              break;
            } else { // マッチしない場合
              posts[i].customData.display = false;
            }
            this.$emit('submit', posts[i].customData.display);
          }
        }
      } else {
        // `All` を選択した場合
        for (var i = 0; i < posts.length; i++) { // 投稿ごとのループ
          posts[i].customData.display = true; // すべての投稿の `display` を `true` に
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
header {
  nav {
    margin-left: auto;
    ul {
      display: flex;
      list-style: none;
      li {
        font-size: 13px;
        line-height: 1.2;
        margin-right: 20px;
        &:last-child {
          margin-right: 0;
        }
        a {
          color: #ccc;
          text-decoration: none;
          &.is-selected {
            color: #222;
          }
        }
      }
    }
  }
}
</style>
