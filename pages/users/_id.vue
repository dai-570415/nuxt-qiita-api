<template>
  <section class="container">
    <nuxt-link class="navi" :to="`/`">&gt;Top</nuxt-link>
    <h2>{{ user.id }}さん 投稿一覧</h2>

    <ul class="item-lists">
      <li class="item-list" v-for="item in items" :key="item.id">
        <h3>
          <span><a :href="item.url" target="_blank">{{ item.title }}</a></span>
          <p>
            by <nuxt-link :to="`/users/${ item.user.id }`">{{ item.user.id }}</nuxt-link>
          </p>
        </h3>
        <h4>{{ item.body.slice(0, 100) }}.....</h4>
      </li>
    </ul>
    <p class="article-write-user">記事を書いた人</p>
    <aside class="user">
        <img :src="user.profile_image_url" alt="Profile Image">
        <div class="user-detail">
            <div class="user-name">{{ user.name }}</div>
            <div class="user-id">@{{ user.id }}</div>
            <p class="user-description">{{ user.description || 'No description' }}</p>
        </div>
    </aside>
  </section>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
    head() {
        return {
            title: this.user.id
        }
    },

    // Vuex使用なし
    // async asyncData({ route, app }) {
    //     const user = await app.$axios.$get(`https://qiita.com/api/v2/users/${route.params.id}`);
    //     const items = await app.$axios.$get(`https://qiita.com/api/v2/items?query=user:${route.params.id}`);
    //     return {
    //         user,
    //         items,
    //     }
    // }

    // Vuex
    async asyncData({ route, store, redirect }) {
        if(store.getters['users'][route.params.id]) {
            return
        }
        try {
            await store.dispatch('fetchUserInfo', { id: route.params.id })
        } catch (e) {
            redirect('/')
        }
    },
    computed: {
        user() {
            return this.users[this.$route.params.id]
        },
        items() {
            return this.userItems[this.$route.params.id] || []
        },
        ...mapGetters(['users', 'userItems'])
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
    h2 {
        font-size: 20px;
        font-weight: 500;
    }
    .container {
        width: 60%;
        margin: 50px auto;
    }
    .container .navi {
        font-size: 18px;
        text-decoration: none;
        color: rgb(0, 199, 0);
        top: 10px;
        left: 10px;
        position: fixed;
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
    .article-write-user {
        font-size: 15px;
        font-weight: bold;
        letter-spacing: -0.25px;
        margin: 70px 0 0 0;
    }
    .user {
        display: flex;
        width: 100%;
        margin: 10px auto 0;
        padding: 30px;
        background: #fff;
    }
    .user img {
        width: 125px;
        height: 125px;
        object-fit: cover;
        margin: 0 20px 0 0;
    }
    .user .user-detail .user-name {
        font-size: 25px;
        font-weight: bold;
    }
    .user .user-detail .user-id {
        font-size: 12px;
        font-weight: 100;
        margin: 0 0 20px 0;
    }
    .user .user-detail .user-description {
        text-align: justify;
        font-size: 14px;
        line-height: 1.5em;
    }

    @media screen and (max-width:768px) {
        body {
            padding: 0;
        }
        h2 {
            padding: 0 0 0 30px;
        }
        .container {
            width:100%;
            margin: 50px auto 0;
        }
        .article-write-user {
            padding: 0 0 0 30px;
        }
        .user {
            display: block;
            text-align: center;
            width: 85%;
            margin: 10px auto 30px;
            padding: 20px;
        }
        .user img {
            border-radius: 50%;
        }
    }
</style>
