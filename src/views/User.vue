<template>
  <div class="user">
    <h1>This is a user page</h1>
    <v-app>
      <v-content>
        <div v-for="(user, i_user) in users" :key="i_user">
          <v-avatar>
            <img :src="user.picture.thumbnail">
          </v-avatar>
          <span>
            {{ user.name.first }}
          </span>
        </div>
      </v-content>
    </v-app>
  </div>
</template>

<script>
  const algoliasearch = require('algoliasearch/lite')
  const config = require('../../algolia.config.js').default

  export default {
    name: 'user',
    data: () => {
      return {
        users: [],
        index: null,
      }
    },
    created: function () {
      const self = this;
      // APIクライアント初期化
      const searchClient = algoliasearch(config.APP_ID, config.API_KEY)
      // userインデックス初期化
      self.index = searchClient.initIndex('user')
      this.searchUser()
    },
    methods: {
      searchUser: function () {
        const self = this;

        // 検索実行
        self.index.search('mr')
          .then(({ hits } = {}) => {
            self.users = hits
          }).catch(err => {
            console.error(err)
            console.error(err.debugData)
        })
      }
    }
  }
</script>