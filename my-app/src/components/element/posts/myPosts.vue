<template>
  <section>
    <ul class="post_category">
      <li v-for="(category,index) in categories" :key="index">
        <a href="javascript:void(0);" @click="filterCategory" :data-category="category.name" :class="['naviLink' , { 'is-selected': category.selected }]">{{category.name}}</a>
      </li>
    </ul>
    <ul class="post_list">
      <li v-for="(post,index) in posts" :key="index" v-show="post.customData.display">
        <a :href="post.acf.post_url" target="_blank">
          <figure><img :src="post.images.full" :alt="post.title.rendered"></figure>
          <div class="wrap" :style="{ color: post.acf.post_color_letter, background: post.acf.post_color_bg }">
            <div class="inner">
              <h2>{{post.title.rendered}}</h2>
              <p>{{post.category_name}}</p>
            </div>
          </div>
        </a>
      </li>
    </ul>
  </section>
</template>

<script>
import axios from "axios";

export default {
  name: 'myPosts',
  data () {
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
  created: function(){
    this.request();
  },
  methods: {
    request: function(){
      axios.get( 'https://works.yuheijotaki.com/wp-json/wp/v2/posts?per_page=100' )
      .then( response => {
        this.posts = response.data;
        // console.log(this.posts);
      })
      .catch( error => {
        console.log(error);
      });
    },
    filterCategory: function(event) { // カテゴリーがクリックされたとき用のメソッド
      // 全体のナビゲーションのクラス削除
      var targetElements = document.getElementsByClassName('naviLink');
      [].forEach.call(targetElements, function(elem) {
        elem.classList.remove('is-selected');
      });
      // 選択したナビゲーションのクラス付与
      event.currentTarget.classList.add('is-selected');
      const selectedCategory = event.currentTarget.getAttribute('data-category'); // クリックしたカテゴリーの取得
      const posts = this.posts;
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
            // this.$emit('send-posts', posts[i].customData.display);
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
.post_category {
  margin-top: 40px;
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
.post_list {
  margin-top: 40px;
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  li {
    width: 25%;
    a {
      display: block;
      text-decoration: none;
      position: relative;
      figure {
        font-size: 0;
        line-height: 0;
        img {
          width: 100%;
          height: auto;
        }
      }
      .wrap {
        display: none;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        .inner {
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          width: 100%;
          padding: 0 10px;
          box-sizing: border-box;
          text-align: center;
          letter-spacing: 0.025em;
          h2 {
            font-size: 13px;
            line-height: 1.2;
          }
          p {
            margin-top: .2em;
            font-size: 10px;
            line-height: 1.4;
          }
        }
      }
      &:hover {
        .wrap {
          display: block;
        }
      }
    }
  }
}
</style>
