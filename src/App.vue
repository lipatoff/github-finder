<template>
  <div class="wrapper">
    <header>
      <div class="navbar">
        <div class="container">
          <div class="navbar-content">
            <div class="logo">GitHub Finder</div>
            <!-- <ul class="navbar-list">
              <li class="navbar-item" v-for="link in links" :key="link.title">
                <router-link class="navbar-link" :title="link.title" :to="link.url">{{ link.title }}</router-link>
              </li>
            </ul> -->
          </div>
        </div>
      </div>
    </header>

    <router-view></router-view>

    <!-- <footer></footer> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      links: [
        { title: 'Home', url: '/' }
      ]
    }
  }
}
</script>

<style lang="scss">
.btn{
  margin: 10px;
}

.navbar-link{
  &.router-link-exact-active{
    color: blue;
  }
}
</style>
