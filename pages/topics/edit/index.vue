<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <h2>Edit Topic {{ $route.params.id  }}</h2>
          <form @submit.prevent="HandleUpdate">
            <div class="form-group mt-5">
              <input type="text" class="form-control" v-model="topic.title" />
              <small v-if="errors.title" class="form-text text-danger">{{
                errors.title[0]
              }}</small>
            </div>
            <button class="btn btn-info btn-sm" type="submit" >
              Update
            </button>
          </form>
          <p class="mt-5">
            <nuxt-link to="/topics">Back to topics</nuxt-link>
          </p>
        </div>
      </div>
      <!-- end of row -->
    </div>
    <!-- end of container -->
  </div>
  <!-- end of div -->
</template>

<script>
import Navbar from "@/components/Navbar";
export default {
  components: { Navbar },
  data() {
      return {
          topic: {
              title: ''
          }
      }
  },
  async asyncData({$axios, params}) {
      const {data} = await $axios.$get(`/topics/${params.id}`);
      return { topic: data }
  },
  methods: {
      async HandleUpdate() {
        await this.$axios.patch(`/topics/${this.$route.params.id}`, {
            title: this.topic.title
        });
        this.$router.push('/topics');
      },
  }
};
</script>

<style></style>
