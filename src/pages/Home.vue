<template>
    <div class="wrapper-content wrapper-content--fixed">
      <section>
        <div class="container">

          <!-- errors -->
          <div class="error" v-if="error" style="margin-bottom: 20px">
            <p>{{ error }}</p>
          </div>

          <!-- search -->
          <search
            :value="search"
            placeholder="Type username..."
            @search="search = $event"/>

          <!-- buttons -->
          <button v-if="!repos" class="btn btnPrimary" @click="getRepos">Search!</button>
          <button v-else class="btn btnPrimary" @click="getRepos">Search Again!</button>

          <!-- user info -->
          <userInfo v-if="repos" :user="user"/>

          <!-- repo items -->
          <repoItems v-if="repos" :repos="repos"/>

        </div>
      </section>
    </div>
</template>

<script>
import search from '@/components/Search.vue'
import userInfo from '@/components/UserInfo.vue'
import repoItems from '@/components/RepoItems.vue'
import axios from 'axios'

export default {
  components: {
    search,
    repoItems,
    userInfo
  },
  data() {
    return {
      search: 'github',
      error: null,
      repos: null,
      user: null
    }
  },
  mounted() {
    this.getRepos()
  },
  methods: {
    getRepos() {
      this.repos = null

      axios.all([
        axios.get(`https://api.github.com/users/${this.search}`),
        axios.get(`https://api.github.com/users/${this.search}/repos`),
        ])
          .then(axios.spread( (res_user, res_repos) => {
            this.error = null
            this.user = res_user.data
            this.repos = res_repos.data
            this.user.repos_length = res_repos.data.length
          }))
          .catch(err => {
            console.log(err)
            this.error = 'Cat`t find this user'
          })
    }
  }
}
</script>

<style lang="scss">
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}
button {
  margin-top: 10px;
}
</style>
