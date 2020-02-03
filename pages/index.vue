<template>
  <div>
    <!-- render data of the person -->
    <!-- render blog posts -->
    <section class="body-container">
      <ul class="items-list wrapper">
        <li class="item" v-for="post in posts" :key="post">
          <article-preview :post="post"></article-preview>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
  import {createClient} from '~/plugins/contentful.js'
  import ArticlePreview from '~/components/article-preview.vue'

  const client = createClient()

  export default {
  asyncData ({env}) {
    return Promise.all([
      client.getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
    ]).then(([posts]) => {
      return {
        posts: posts.items
      }
    }).catch(console.error)
  },
  components: {
    ArticlePreview
  }
}
</script>
