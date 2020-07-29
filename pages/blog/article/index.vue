<template>
  <NuxtChild />
</template>

<script>
export default {
  name: 'Article',
  fetch({ redirect }) {
    redirect('/blog');
  },
}
</script>
