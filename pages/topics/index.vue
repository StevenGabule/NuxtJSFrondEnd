<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <h1>Latest Topics</h1>
          <div
            v-for="(topic, index) in topics"
            :key="index"
            class="bg-light mt-5 mb-5"
            style="padding: 20px"
          >
            <h2>
              <nuxt-link
                :to="{ name: 'topics-id', params: { id: topic.id } }"
                >{{ topic.title }}</nuxt-link
              >
            </h2>
            <div v-if="authenticated">
              <div v-if="user.id === topic.user.id">
                <nuxt-link :to="{ name: 'topics-edit', params: { id: topic.id } }" class="btn btn-info btn-sm">Edit</nuxt-link>
                <button @click="DeleteTopic(topic.id)" class="btn btn-danger btn-sm">delete</button>
              </div>
            </div>
            <p class="text-muted">
              {{ topic.created_at }} by {{ topic.user.name }}
            </p>
            <div
              v-for="(content, index) in topic.posts"
              :key="index"
              class="ml-5 content"
            >
              {{ content.body }}
              <p class="text-muted">
                {{ content.created_at }} by {{ content.user.name }}
              </p>
            </div>
          </div>
          <nav>
            <ul class="pagination justify-content-center">
              <li v-for="(key, value) in links" class="page-item" :key="value">
                <a href="#" @click="loadMore(key)" class="page-link">{{
                  value
                }}</a>
              </li>
            </ul>
          </nav>
        </div>
        <!-- end of col-lg-12 -->
      </div>
      <!-- end of row -->
    </div>
    <!-- end of container -->
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
export default {
  components: { Navbar },
  data() {
    return {
      topics: [],
      links: []
    };
  },
  async asyncData({ $axios }) {
    let { data, links } = await $axios.$get("/topics");
    return { topics: data, links };
  },
  methods: {
    async loadMore(key) {
      let { data } = await this.$axios.$get(key);
      return (this.topics = { ...this.topics, ...data });
    },
    async DeleteTopic(topicId) {
      try {
        await this.$axios.delete(`/topics/${topicId}`);
        console.info("Deleted topic");
        this.$router.push('/topics');
      } catch(e) {}
    }
  }
};
</script>
<style scoped>
.content {
  border-left: 10px solid white;
  padding: 0 10px 0 10px;
}
</style>
