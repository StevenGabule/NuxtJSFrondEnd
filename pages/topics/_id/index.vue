<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="bg-light mt-5 mb-5" style="padding: 20px">
        <h2>{{ topic.title }}</h2>
        <p class="text-muted">
          {{ topic.created_at }} by {{ topic.user.name }}
        </p>
        <div
          v-for="(content, index) in topic.posts"
          :key="index"
          class="ml-5 content"
        >
          <p>{{ content.body }}</p>
          <div v-if="authenticated">
            <div v-if="user.id === content.user.id">
              <nuxt-link class="btn btn-info btn-sm" :to="{ name: 'topics-posts-edit', params: { id: content.id } }">Edit</nuxt-link>
              <button @click="DeletePost(content.id)" class="btn btn-danger btn-sm">Delete</button>
            </div>
          </div>
          <p class="text-muted">
            {{ content.created_at }} by {{ content.user.name }}
          </p>
        </div>
      </div>
      <!-- end of bg-light -->

      <div class="mt-5 ml-5" v-if="authenticated">
        <form @submit.prevent="create">
          <div class="form-group">
            <label for="">Add New Post</label>
            <textarea
              v-model="body"
              class="form-control"
              placeholder="Write something..."
              rows="5"
            ></textarea>
            <small class="form-text text-danger" v-if="errors.body">{{
              errors.body[0]
            }}</small>
          </div>
          <button class="btn btn-outline-primary btn-sm" type="submit">
            Add A New Post
          </button>
        </form>
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
      topic: "",
      body: ""
    };
  },
  async asyncData({ $axios, params }) {
    const { data } = await $axios.$get(`/topics/${params.id}`);
    return { topic: data };
  },
  methods: {
    async create() {
      try {
        await this.$axios.$post(`/topics/${this.$route.params.id}/posts`, {
          body: this.body
        });
        this.$router.push("/topics");
      } catch (e) {}
    },
    async DeletePost(id) {
        await this.$axios.$delete(`/topics/${this.$route.params.id}/posts/${id}`);
        this.$router.push('/topics');
    }
  }
};
</script>
