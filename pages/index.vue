<template>
  <main>
    <div
      class="w-full h-screen bg-cover bg-center bg-no-repeat bg-fixed"
      :style="`background-image: url(${index.backgroundImage})`"
    >
      <div
        class="
          grid
          gap-4
          justify-center
          content-center
          w-full
          h-full
          bg-black bg-opacity-80 bg-fixed
          i-bg-wrap
          text-center
          relative
        "
      >
        <span class="text-8xl text-white">{{ index.title }}</span>
        <span class="text-xl text-gray-50">{{ index.subtitle }}</span>

        <span class="absolute left-0 right-0 bottom-10 text-white text-5xl"
          ><i class="ri-arrow-down-s-line"></i
        ></span>
      </div>
    </div>

    <div class="grid grid-cols-12 px-6 py-4 bg-gray-50">
      <aside class="col-span-full md:col-span-3 lg:col-span-3 xl:col-span-2">
        <header
          class="
            grid
            py-4
            m-6
            md:mx-auto
            sticky
            top-5
            bg-white
            rounded-xl
            shadow-sm
          "
        >
          <div
            class="
              w-40
              h-40
              justify-self-center
              border-solid border-2 border-white
              rounded-full
              overflow-hidden
              text-center
              shadow-lg
            "
          >
            <img :src="index.avatar" alt="Avatar" />
          </div>

          <span class="py-6 tracking-wide text-gray-900 text-xl text-center">{{
            index.name
          }}</span>

          <div class="grid py-8 justify-center bg-gray-50">
            <div v-for="(profile, index) of index.profiles" :key="index">
              <i
                :class="`ri-${profile.icon}`"
                class="mr-1 align-middle font-light text-gray-600"
              ></i>
              <span class="align-middle text-sm text-gray-900 tracking-wide">{{
                profile.content
              }}</span>
            </div>
          </div>

          <nav class="grid gap-2 py-4 text-center">
            <NuxtLink
              v-for="link of links"
              :key="link.to"
              :to="link.to"
              class="
                py-4
                mx-6
                transition
                duration-200
                ease-linear
                text-lg text-gray-700
                bg-white
                hover:bg-gray-100
                rounded-2xl
              "
              >{{ link.name }}</NuxtLink
            >
          </nav>
        </header>
      </aside>

      <article class="col-span-full md:col-span-9 lg:col-span-9 xl:col-span-8">
        <section
          v-for="post of posts"
          :key="post.slug"
          class="grid gap-4 m-6 p-6 bg-white rounded-xl shadow-sm"
        >
          <NuxtLink
            :to="`/post/${post.slug}`"
            class="font-semibold truncate text-gray-900 text-center text-xl"
            >{{ post.title }}</NuxtLink
          >

          <div class="grid grid-flow-col justify-center">
            <NuxtLink
              v-for="tag of post.tags"
              :key="tag"
              :to="`/tags/${tag}`"
              class="
                px-1
                py-0.5
                mx-2
                transition
                duration-100
                ease-linear
                text-sm text-gray-500
                border border-gray-500
                hover:bg-gray-100
                rounded
              "
            >
              {{ tag }}
            </NuxtLink>
          </div>

          <p>{{ post.description }}</p>

          <div class="grid gap-1 text-sm text-gray-600 tracking-wide">
            <div>
              <i class="mr-1 align-middle ri-calendar-line"></i>
              <span class="align-middle"
                >Created at: {{ formatDate(post.createdAt) }}</span
              >
            </div>

            <div>
              <i class="mr-1 align-middle ri-time-line"></i>
              <span class="align-middle"
                >Updated at: {{ formatDate(post.updatedAt) }}</span
              >
            </div>
          </div>
        </section>

        <section class="py-8 text-gray-500 text-center text-sm">
          &copy; 2021 · Design By Sxsw · Made with ❤
        </section>
      </article>

      <aside class="hidden xl:block col-span-2">
        <div
          class="
            grid
            gap-2
            py-4
            my-6
            sticky
            top-5
            bg-white
            rounded-xl
            shadow-sm
            text-4xl text-gray-700 text-center
          "
        >
          <a
            v-for="(social, index) of index.socials"
            :key="index"
            :href="social.link"
            target="_blank"
            class="py-4 hover:bg-gray-100"
            ><i :class="`ri-${social.icon}`"></i
          ></a>
        </div>
      </aside>
    </div>
  </main>
</template>

<script>
import moment from 'moment'

export default {
  async asyncData({ $content }) {
    const index = await $content('pages/index').fetch()
    const posts = await $content('posts')
      .only(['slug', 'description', 'title', 'tags', 'createdAt', 'updatedAt'])
      .sortBy('updatedAt', 'asc')
      .fetch()

    return {
      index,
      posts,
    }
  },
  data() {
    return {
      links: [
        {
          name: 'Posts',
          to: '/posts',
        },
        {
          name: 'Tags',
          to: '/tags',
        },
      ],
    }
  },
  methods: {
    formatDate: (date) => {
      return moment(date).format('YYYY/MM/DD - HH:mm:ss')
    },
  },
}
</script>

<style lang="postcss" scoped>
.i-bg-wrap {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='6' height='6' viewBox='0 0 4 4'%3E%3Cpath fill='%23000000' fill-opacity='0.4' d='M1 3h1v1H1V3zm2-2h1v1H3V1z'%3E%3C/path%3E%3C/svg%3E");
  backdrop-filter: saturate(100%) blur(1px);
}
</style>
