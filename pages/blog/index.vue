<template>
  <div class="h-full">
    <section class="container py-4">
      <h1 class="text-5xl pb-2">Blog</h1>
      <div class="w-full">
        <ul>
          <li v-for="item in articles" :key="item.id">
            <div
              v-if="item.is_release"
              class="rounded overflow-hidden shadow-lg bg-white my-4 cursor-pointer"
              @click="goToDetailPost(item.id)"
            >
              <img
                class="w-full h-64 object-cover transition duration-500 ease-in-out transform hover:-translate-y-1 hover:scale-105"
                :src="`https://jamstack-api.herokuapp.com${item.image.url}`"
                :alt="item.title"
              >
              <div class="px-4 py-4">
                <h1 class="text-2xl font-medium">{{ item.title }}</h1>
                <span class="text-sm">Author : {{ item.author }} - Date : {{ formatDate(item.date) }}</span>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import moment from 'moment'
import { getAllArticles } from '@/gql/article.gql';

export default {
  name: 'Blog',
  apollo: {
    articles: {
      query: getAllArticles,
    }
  },
  methods: {
    goToDetailPost(id) {
      this.$router.push(`/blog/article/${id}`);
    },
    formatDate(date) {
      return moment(date).format('dddd, DD MM YYYY');
    }
  },
}
</script>
