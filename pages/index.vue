<template>
  <section class="container">
    <h1><strong>初心者必見!</strong> 初めてのAPI通信 ~Nuxt.js編~</h1>
    <h2>[ Nuxt ] 投稿一覧</h2>
    <ul class="item-lists">
      <li class="item-list" v-for="item in items" :key="item.id">
        <h3>
          <span><a :href="item.url" target="_blank">{{ item.title }}</a></span>
          <p>by <nuxt-link :to="`/users/${ item.user.id }`">{{ item.user.id }}</nuxt-link></p>
        </h3>
        <h4>{{ item.body.slice(0, 100) }}.....</h4>
      </li>
    </ul>
  </section>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  // Vuex使用なし
  // async asyncData({ app }) {
  //   const items = await app.$axios.$get('https://qiita.com/api/v2/items?query=tag:nuxt.js');
  //   return {
  //     items
  //   }
  // }

  // Vuex
  async asyncData({ store }) {
    if (store.getters['items'].length) { // length = indexに+1したものを返す
      return
    }
    await store.dispatch('fetchItems');
  },
  computed: {
    ...mapGetters(['items'])
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    border-top: 5px solid rgb(0, 199, 0);
    padding: 0 0 100px 0;
    background: #efefef;
  }
  a {
    color: crimson;
  }
  ul li {
    list-style: none;
  }
  h1 {
    text-align: center;
    font-size: 35px;
    line-height: 1.25em;
    margin: 0 0 50px 0;
    padding: 0 0 5px 0;
    transform: rotate(-5deg);
    color: rgb(0, 199, 0);
    border-bottom: 2px solid rgb(0, 199, 0);
    right: 70px;
    position: relative;
  }
  h2 {
    font-size: 20px;
    font-weight: 500;
  }
  .container {
    width: 60%;
    margin: 50px auto;
  }
  .item-lists {
    margin: 15px 0 0 0;
  }
  .item-lists .item-list {
    padding: 25px 30px 15px;
    border-bottom: 1px solid #ccc;
    background: #fff;
  }
  .item-lists .item-list:last-child {
    border-bottom: none;
  }
  .item-lists .item-list h3 {
    font-size: 18px;
    font-weight: 500;
    line-height: 1.5em;
    letter-spacing: -0.25px;
    margin: 0 0 10px 0;
  }
  .item-lists .item-list h3 p {
    font-size: 12px;
    font-weight: 100;
  }
  .item-lists .item-list h3 p a {
    font-size: 15px;
    color: rgb(0, 82, 0);
    text-decoration: none;
  }
  .item-lists .item-list h4 {
    font-size: 15px;
    font-weight: 300;
    line-height: 1.75em;
    margin: 0 0 15px 0;
  }

  @media screen and (max-width:768px) {
    body {
      padding: 0;
    }
    h1 {
      font-size: 25px;
      padding: 0 15px 15px;
      right: 0px;
      transform: rotate(0deg);
    }
    h2 {
      padding: 0 0 0 30px;
    }
    .container {
      width:100%;
      margin: 50px auto 0;
    }
  }
</style>
