<script setup>
/* Ane' Burger - 24565068 */
import { ref, onMounted, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const config = useRuntimeConfig()
const route = useRoute()
const router = useRouter()

const posts = ref([])
const searchQuery = ref(route.query.q || '')

const searchPosts = async () => {
  const q = route.query.q || ''
  if (!q) {
    posts.value = []
    return
  }

  const query = encodeURIComponent(q)
  try {
    const res = await fetch(
      `${config.public.strapiUrl}/api/blog-posts?filters[$or][0][Title][$containsi]=${query}&filters[$or][1][Author][$containsi]=${query}`
    )
    const data = await res.json()
    posts.value = data.data || []
  } catch (err) {
    console.error('Search failed:', err)
    posts.value = []
  }
}

watch(searchQuery, (newVal) => {
  router.replace({ query: { q: newVal } })
})

watch(() => route.query.q, (newQ) => {
  searchQuery.value = newQ || ''
  searchPosts()
})

onMounted(searchPosts)
</script>

<template>
  <div>
    <h1>Search</h1>
    <input v-model="searchQuery" type="text" placeholder="Search by author or title..." />
    <p v-if="!searchQuery" id="looking"><img src="../assets/search.png" height="80" id="search"/>Looking for a specific blog post?</p>
    <p v-else-if="searchQuery && posts.length === 0" id="noposts">No blog posts found for your search.</p>
    <ul v-else>
      <li v-for="post in posts" :key="post.id">
        <NuxtLink :to="`/blog/${post.slug}`">{{ post.Title }}</NuxtLink>
        <p id="content">By {{ post.Author }}</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@300..700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic&display=swap');

h1 {
    font-family: 'Comfortaa';
    font-size: 2.5em;
    color: #DB2B39;
    margin-left: 50px;
    margin-top: 30px;
    font-weight: 700;
    border-bottom: 1px solid #DB2B39;
    padding-bottom: 30px;
    width: 350px;
}

input {
    font-family: 'Comfortaa';
    border: 1px solid #DB2B39;
    color: #DB2B39;
    font-weight: 600;
    padding: 13px;
    border-radius: 30px;
    margin-left: 50px;
    margin-top: 10px;
    margin-bottom: 50px;
    width: 323px;
}

#noposts {
    font-family: 'Comfortaa';
    color: black;
    display: flex;
    margin-top: 30px;
    justify-content: center;
    align-items: center;
    font-weight: 700;
}

#looking {
    font-family: 'Comfortaa';
    color: black;
    display: flex;
    margin-top: 30px;
    justify-content: center;
    align-items: center;
    font-weight: 800;
    font-size: 1.4em;
}

ul {
    margin: auto;
    width: 700px;
    list-style-type: none;
    font-family: 'Comfortaa';
}

ul li a {
    text-decoration: none;
    color: #DB2B39;
    font-size: 1.5em;
    font-weight: 700;
}

#content {
    margin-bottom: 45px;
}

</style>
