<template>
  <section class="container">
    <div>
      <h3>{{user.id}}</h3>
      <img :src="user.profile_image_url" width="120" alt="">
      <p>{{user.description || 'No description'}}</p>
      <p>
        <nuxt-link to="/">
          <small><b>トップへ戻る</b></small>
        </nuxt-link>
      </p>
      <h3>{{user.id}}さんの投稿一覧</h3>
      <ul>
        <li v-for="item in items" :key="item.id">
          <h4>
            <span>{{item.title}}</span>
          </h4>
          <p><a target="_blank" :href="item.url">{{item.url}}</a></p>
          <div>{{item.body.slice(0,130)}}.....</div>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  head() {
    return {
      title: this.user.id
    }
  },
  async asyncData({ route, store, redirect }) {
    if (store.getters['user'].length) {
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
      return this.user
    },
    items() {
      return this.userItems[this.$route.params.id] || []
    },
    ...mapGetters(['user', 'userItems'])
  }
}
</script>
