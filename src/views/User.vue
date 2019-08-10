<template>
  <div class="user">
    <h1>This is a user page</h1>
    <v-app>
      <v-content>
        <v-form>
          <v-container>
            <v-flex>
              <v-text-field
                label="Search Keyword"
                v-model="search_keyword"></v-text-field>
            </v-flex>
          </v-container>
        </v-form>

        <div v-for="(user, i_user) in users" :key="i_user">
          <v-avatar>
            <img :src="user.picture.thumbnail"
                  :alt="user.name.last + ' ' + user.name.first">
          </v-avatar>
          <span>
            {{ user.name.last }} {{ user.name.first }}
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
        search_keyword: '',
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
    watch: {
      search_keyword: function () {
        this.searchUser()
      }
    },
    methods: {
      searchUser: function () {
        const self = this;

        // 検索実行
        self.index.search(self.search_keyword)
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