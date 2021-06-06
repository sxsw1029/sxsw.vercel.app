<template>
  <main>
    <nuxt-content :document="page" />
  </main>
</template>
<script>
export default {
  async asyncData({ params, $content }) {
    const slug = await params.slug
    const page = await $content(`posts/${slug}`).fetch()
    return { page }
  },
}
</script>
