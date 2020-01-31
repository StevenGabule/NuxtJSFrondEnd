<template>
  <div class="container col-md-6 mt-5">
    <h2>Login</h2>
    <form @submit.prevent="submit">
      <div class="form-group">
        <label>Email address</label>
        <input
          type="email"
          class="form-control"
          placeholder="Enter email" 
          v-model.trim="form.email" 
          autofocus
        />
        <small class="form-text text-danger" v-if="errors.email">{{ errors.email[0] }}</small>
      </div>
      <div class="form-group">
        <label>Password</label>
        <input
          type="password"
          class="form-control"
          placeholder="Enter password"
          v-model.trim="form.password"
        />
        <small class="form-text text-danger" v-if="errors.password">{{ errors.password[0] }}</small>
      </div>

      <button type="submit" class="btn btn-primary">Login</button>
    </form>
    <p>Don't have an account? <nuxt-link to="/register">Register</nuxt-link> | <nuxt-link to="/">Home</nuxt-link></p>
  </div>
</template>

<script>
export default {

  middleware: ['guest'],

  data() {
    return {
      form: {
        email: '',
        password: '',
        error: null
      }
    }
  },
  methods: {
    async submit() {
      try {
        await this.$auth.loginWith("local", { 
        data: this.form 
        });
        this.$router.push('/dashboard');
      } catch(e) {
      }
    }
  }
};
</script>

<style></style>
