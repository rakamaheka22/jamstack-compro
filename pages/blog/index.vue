<template>
  <div>
    <section class="container py-4">
      <div class="w-full">
        <ul>
          <li v-for="item in articles" :key="item.id">
            <div v-if="item.is_release" class="rounded overflow-hidden shadow-lg bg-white my-4 cursor-pointer" @click="goToDetailPost(item.id)">
              <img class="w-full h-64 object-cover transition duration-500 ease-in-out transform hover:-translate-y-1 hover:scale-105" :src="`http://localhost:1337${item.image.url}`" :alt="item.title">
              <div class="px-4 py-4">
                <h1 class="text-2xl font-medium">{{ item.title }}</h1>
                <span class="text-sm">Author : {{ item.author }} - Date : {{ item.date }}</span>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import { getAllArticles } from '@/gql/article.gql';
import Logo from "~/components/Logo.vue";

export default {
  name: 'Blog',
  components: {
    Logo
  },
  apollo: {
    articles: {
      query: getAllArticles,
    }
  },
  methods: {
    goToDetailPost(id) {
      this.$router.push(`/blog/article/${id}`);
    },
  },
}
</script>
