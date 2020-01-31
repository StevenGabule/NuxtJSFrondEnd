<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="bg-light mt-5 mb-5" style="padding: 20px">
        <h2>{{ topic.title }}</h2>
        <p class="text-muted">{{ topic.created_at }} by {{ topic.user.name }}</p>
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
      
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";

export default {
  components: { Navbar },
  data() {
    return {
      topic: ''
    };
  },
  async asyncData({$axios, params}) {
    const {data} = await $axios.$get(`/topics/${params.id}`);
    return { topic: data }
  }
};
</script>

<style></style>
