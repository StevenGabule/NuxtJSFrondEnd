<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col-lg-12 mt-5">
          <h2 class="h2 text-capitalize">Topic post edit page</h2>
          <form @submit.prevent="update">
            <div class="form-group">
              <label for=""></label>
              <textarea
                v-model="post.body"
                class="form-control"
                rows="10"
              ></textarea>
              <small v-if="errors.body" class="form-text text-danger">{{
                errors.body[0]
              }}</small>
            </div>
            <button class="btn btn-outline-success btn-sm" type="submit">
              Update
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
export default {
  components: { Navbar },
  data() {
    return {
      post: {
        body: ""
      }
    };
  },

  async asyncData({ $axios, params }) {
    const { data } = await $axios.$get(
      `/topics/${params.id}/posts/${params.body}`
    );
    return { post: data };
  },

  methods: {
    async update() {
      await this.$axios.$patch(
        `/topics/${this.$route.params.id}/posts/${this.$route.params.body}`,
        {
          body: this.post.body
        }
      );
      this.$router.push("/topics");
    }
  }
};
</script>

<style></style>
