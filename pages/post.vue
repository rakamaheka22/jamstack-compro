<template>
  <div class="rounded overflow-hidden shadow-lg bg-white mx-4 my-4 py-4 px-4">
    <h1 class="text-2xl font-medium mb-10">Add Article</h1>

    <form
      class="w-full max-w-xl"
    >
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/4">
          <label class="block text-gray-500 font-bold mb-1 md:mb-0 pr-4" for="inline-full-name">
            Title
          </label>
        </div>
        <div class="md:w-3/4">
          <input v-model="article.title" class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-teal-500" id="inline-full-name" type="text">
        </div>
      </div>
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/4">
          <label class="block text-gray-500 font-bold mb-1 md:mb-0 pr-4" for="inline-full-name">
            Author
          </label>
        </div>
        <div class="md:w-3/4">
          <input v-model="article.author" class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-teal-500" id="inline-full-name" type="text">
        </div>
      </div>
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/4">
          <label class="block text-gray-500 font-bold mb-1 md:mb-0 pr-4" for="inline-username">
            Content
          </label>
        </div>
        <div class="md:w-3/4">
          <textarea v-model="article.content" class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-teal-500" id="inline-username" rows="10"></textarea>
        </div>
      </div>
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/4">
          <label class="block text-gray-500 font-bold mb-1 md:mb-0 pr-4" for="inline-username">
            Publish?
          </label>
        </div>
        <div class="md:w-3/4">
          <input v-model="article.is_release" type="radio" id="male" value="1">
          <label for="male">Yes</label><br>
          <input v-model="article.is_release" type="radio" id="female" value="0">
          <label for="female">No</label><br>
        </div>
      </div>
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/4">
          <label class="block text-gray-500 font-bold mb-1 md:mb-0 pr-4" for="inline-username">
            Image
          </label>
        </div>
        <div class="md:w-3/4">
          <input
            class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-teal-500"
            id="inline-full-name"
            type="file"
            @change="onFilePicked"
          >
        </div>
      </div>
      <div class="md:flex md:items-center">
        <div class="md:w-3/3">
          <button
            :disabled="article.title && article.author && article.content ? false : true"
            type="button"
            class="btn-publish"
            @click="submitArticle"
          >
            Publish Article
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { createArticle } from '@/gql/article.gql';
import { uploadFile } from '@/gql/upload.gql';

export default {
  name: 'Post',
  data() {
    return {
      article: {
        title: null,
        author: null,
        content: null,
        is_release: false,
        image: null,
      },
      imgFile: null,
    };
  },
  methods: {
    async onFilePicked(ev) {
      console.log(ev);
      this.imgFile = ev.target.files[0];
      await this.$apollo.mutate({
        mutation: uploadFile,
        variables: {
          file: this.imgFile,
        },
      })
      .then((data) => {
        console.log(data);
      });
    },
    submitArticle() {

    }
  }
}
</script>

<style type="text/postcss">
.btn-publish {
  @apply bg-teal-500;
  @apply text-white font-bold py-2 px-4 rounded shadow;
}

.btn-publish:hover {
  @apply bg-teal-400;
}

.btn-publish:focus {
  @apply shadow-outline outline-none;
}

.btn-publish:disabled,
.btn-publish:[disabled] {
  @apply opacity-50 cursor-not-allowed;
}
</style>
