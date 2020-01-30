<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">

    
    <nuxt-link class="navbar-brand" to="/">NUXT APP</nuxt-link>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarNav"
      aria-controls="navbarNav"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

   

    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <nuxt-link class="nav-link" to="/">Home</nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link class="nav-link" to="/posts">Posts</nuxt-link>
        </li>
      </ul>
    <template v-if="!authenticated">
        <ul class="navbar-nav ml-auto">
            <li class="nav-item">
            <nuxt-link class="nav-link" to="/login">Login</nuxt-link>
            </li>
            <li class="nav-item">
            <nuxt-link class="nav-link" to="/register">Register</nuxt-link>
            </li>
        </ul>
    </template>

    <template v-else>
        <ul class="navbar-nav ml-auto">
            <li class="nav-item">
                <a href="javascript:void(0)" class="nav-link">{{ user.name }}</a>
            </li>
            <li class="nav-item">
                <a href="javascript:void(0)" class="nav-link" @click.prevent="logout">Logout</a>
            </li>
        </ul>
    </template>
    
    </div>
    </div>
  </nav>
</template>

<script>
export default {
    methods: {
        logout() {
            this.$auth.logout();
        }
    }
};
</script>
