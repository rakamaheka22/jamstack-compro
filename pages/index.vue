<template>
  <div class="h-screen">
    <section class="container py-4">
      <Hero />
    </section>
  </div>
</template>

<script>
import Hero from '~/components/hero';

export default {
  name: 'Index',
  components: {
    Hero,
  }
}
</script>
