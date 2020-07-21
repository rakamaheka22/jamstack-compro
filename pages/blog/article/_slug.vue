<template>
  <div>
    <section class="container py-4">
      <div class="w-full">
        <div v-if="article.is_release" class="rounded overflow-hidden shadow-lg bg-white my-4 cursor-pointer">
          <div class="px-4 py-4">
            <h1 class="text-2xl font-medium">{{ article.title }}</h1>
            <span class="text-sm">Author : {{ article.author }} - Date : {{ article.date }}</span>
            <img class="w-full h-64 object-cover mt-4" :src="`http://localhost:1337${article.image.url}`" :alt="article.title">
            <div class="mt-4 whitespace-pre-line" markdown="1">
              {{ article.content }}
            </div>
          </div>
        </div>
        <h1 class="text-2xl font-medium my-6">Komentar</h1>
        <div class="rounded overflow-hidden shadow-lg bg-white my-4 px-4 py-4">
          <div class="md:flex md:items-center">
            <div class="md:w-2/6 mr-4">
              <input v-model="name" class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-teal-500" id="inline-full-name" type="text" placeholder="Masukkan Nama">
            </div>
            <div class="md:w-3/6">
              <input v-model="message" class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-teal-500" id="inline-full-name" type="text" placeholder="Masukkan Komentar">
            </div>
            <div class="md:w-1/6">
              <button class="shadow bg-teal-500 hover:bg-teal-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded ml-4" type="button" @click="saveComment">
                Post
              </button>
            </div>
          </div>
        </div>
        <div v-if="article.is_release" class="rounded overflow-hidden shadow-lg bg-white my-4 px-4 py-4">
          <ul>
            <li v-for="item in article.comments" :key="item.id">
              <div class="flex mb-4 justify-between">
                <div class="w-2/3">
                  <h1 class="text-l font-medium">{{ item.name }}</h1>
                  <span class="text-sm">{{ item.message }}</span>
                </div>
                <div class="1/3">
                  <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded" @click="actionDelete(item.id)">
                    Delete
                  </button>
                </div>
              </div>
              <hr class="my-2">
            </li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { getArticle } from '@/gql/article.gql';
import { setComment, deleteComment } from '@/gql/comment.gql';
import Logo from "~/components/Logo.vue";

export default {
  name: 'Blog',
  components: {
    Logo
  },
  async asyncData({ app, params }) {
    let article = null;
    const client = await app.apolloProvider.defaultClient;
    await client.query(
      {
        query: getArticle,
        variables: {
          id: params.slug,
        }
      }
    ).then(async ({ data }) => {
        article = await data.article;
    })
    return {
      article,
      idParams: params.slug,
    };
  },
  data() {
    return {
      article: null,
      name: '',
      message: '',
      idParams: 0,
    }
  },
  methods: {
    async loadData() {
      const res = await this.$apollo.query({
        query: getArticle,
        variables: { id: this.idParams }
      });
      this.article = await res.data?.article;
    },
    async saveComment() {
      if (!this.name) this.name = 'Anonymous';
      await this.$apollo.mutate({
        mutation: setComment,
        variables: {
          name: this.name,
          message: this.message,
          article: this.idParams,
        },
        // refetchQueries: [
        //   {
        //     query: getArticle,
        //     variables: { id: this.idParams }
        //   }
        // ]
      });
      this.loadData();
      this.name = '';
      this.message = '';
    },
    async actionDelete(id) {
      await this.$apollo.mutate({
        mutation: deleteComment,
        variables: {
          id,
        },
      });
      this.loadData();
    },
  },
}
</script>
