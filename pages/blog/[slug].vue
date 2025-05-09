<script setup>
/* Ane' Burger - 24565068 */

import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { marked } from 'marked'

const config = useRuntimeConfig()
const route = useRoute()
const post = ref(null)

onMounted(async () => {
  try {
    console.log('Slug:', route.params.slug)
    const res = await fetch(`${config.public.strapiUrl}/api/blog-posts?filters[slug][$eq]=${route.params.slug}&populate=*`)
    const data = await res.json()
    post.value = data.data[0] || null
  } catch (err) {
    console.error('Error fetching post:', err)
  }
})
</script>

<template>
  <div v-if="post" id="postDiv">
    <h1>{{ post.Title }}</h1>
    <p id="author">By {{ post.Author }}</p>
    <div v-html="marked(post.Content)" id="content"></div>
  </div>
  <div v-else>
    <p>Post not found.</p>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@300..700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic&display=swap');

h1 {
    font-family: 'Comfortaa';
    font-size: 1.9em;
    color: #DB2B39;
    margin-top: 20px;
    margin-bottom: 5px;
}

#postDiv {
  margin: auto;
  width: 950px;
  margin-bottom: 350px;
  margin-top: 70px;
}

#author {
    font-family: 'Zen Maru Gothic';
    font-size: 1.2em;
    color: #DB2B39;
    margin-bottom: 50px;
    font-weight: 500;
}

#content {
    font-family: 'Zen Maru Gothic';
    font-size: 1.2em;
    color: black;
    margin-bottom: 50px;
    font-weight: 700;
}

</style>
