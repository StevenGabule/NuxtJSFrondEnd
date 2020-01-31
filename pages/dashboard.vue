<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <h2>Create A Topic</h2>
          <form @submit.prevent="submit">
            <div class="form-group">
              <label for="title">Topic Title</label>
              <input type="text" v-model="form.title" class="form-control" placeholder="Enter the title">
              <small class="form-text text-danger" v-if="errors.title">{{ errors.title[0] }}</small>
            </div>
            <div class="form-group">
              <label for="body">Body</label>
              <textarea v-model="form.body" id="body" rows="10" class="form-control"></textarea>
              <small class="form-text text-danger" v-if="errors.body">{{ errors.body[0] }}</small>

            </div>
            <div class="form-group">
              <button type="submit" class="btn btn-primary">Create</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
export default {
  middleware: ['auth'],
  components: { Navbar },
  data() {
    return {
      form: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    async submit() {
        try {
          await this.$axios.$post('/topics', this.form);
          return this.$router.push('/')
        } catch(e) {

        }
    }
  }
};
</script>
