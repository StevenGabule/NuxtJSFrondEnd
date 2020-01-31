<template>
  <div class="container col-md-6 mt-5">
    <h2>Register</h2>
 

    <form @submit.prevent="submit">
      <div class="form-group">
        <label>Name</label>
        <input
          type="text"
          class="form-control"
          placeholder="Enter name"
          v-model.trim="form.name"
        />
        <small class="form-text text-danger" v-if="errors.name">{{
          errors.name[0]
        }}</small>
      </div>

      <div class="form-group">
        <label>Email address</label>
        <input
          type="email"
          class="form-control"
          placeholder="Enter email"
          v-model.trim="form.email"
        />
        <small class="form-text text-danger" v-if="errors.email">{{
          errors.email[0]
        }}</small>
      </div>
      <div class="form-group">
        <label>Password</label>
        <input
          type="password"
          autocomplete="current-password"
          class="form-control"
          placeholder="Enter password"
          v-model.trim="form.password"
        />
        <small class="form-text text-danger" v-if="errors.password">{{
          errors.password[0]
        }}</small>
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    <p>Have an account already? <nuxt-link to="/login">Login</nuxt-link> | <nuxt-link to="/">Home</nuxt-link></p>
  </div>
</template>

<script>
import Notification from "~/components/Notification";
export default {
  components: {
    Notification,
  },

    middleware: ['guest'],

  data() {
    return {
      form: {
        name: "",
        email: "",
        password: "",
        error: null
      }
    };
  },

  methods: {
    async submit() {
      try {
        await this.$axios.$post("register", this.form);
        await this.$auth.loginWith("local", {
          data: {
            email: this.form.email,
            password: this.form.password
          }
        });
        this.$router.push("/dashboard");
      } catch (e) {
        this.error = e.response.data.message;
        // this.$store.dispatch("validation/setErrors", e.response.data.message);
      }
    }
  }
};
</script>
