<script setup>
/* Ane' Burger - 24565068 */

import { ref, onMounted, computed } from 'vue'

const config = useRuntimeConfig()
const posts = ref([])
const categories = ref([])
const selectedCategory = ref(null)
const searchQuery = ref('')

onMounted(async () => {
  try {
    const res = await fetch(`${config.public.strapiUrl}/api/blog-posts?populate=category`)
    const postData = await res.json()
    posts.value = postData.data || [] 
    console.log(posts.value);
  } catch (err) {
    console.error('Error fetching posts:', err)
    posts.value = []
  }

  try {
    const catRes = await fetch(`${config.public.strapiUrl}/api/categories`)
    const categoryData = await catRes.json()
    categories.value = categoryData.data || []
  } catch (err) {
    console.error('Error fetching categories:', err)
    categories.value = []
  }
})

const filteredPosts = computed(() => {
  return posts.value.filter((post) => {
    if (!selectedCategory.value) return true 
    return post.category?.id === selectedCategory.value
  })
})
</script>

<template>
  <div>
    <h1>Home</h1>

    <select v-model="selectedCategory">
      <option :value="null">All Categories</option>
      <option v-for="cat in categories" :key="cat.id" :value="cat.id">
        {{ cat.name }}
      </option>
    </select>

    <p v-if="filteredPosts.length === 0" id="noposts">No posts exist for this category.</p>

    <!-- <input v-model="searchQuery" type="text" placeholder="Search posts..." /> -->

    <ul v-else> 
      <li v-for="post in filteredPosts" :key="post.id">
        <NuxtLink :to="`/blog/${post.slug}`">
          <!-- <p>{{ post.slug }}</p> -->
          <h2>{{ post.Title }}</h2>
        </NuxtLink>
        <p id="by">By {{ post.Author }}</p>
        <p id="content">{{ post.Content.slice(0, 60) }}...</p>
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

select {
    font-family: 'Comfortaa';
    border: 1px solid #DB2B39;
    color: #DB2B39;
    font-weight: 600;
    padding: 10px;
    border-radius: 30px;
    margin-left: 50px;
    margin-top: 10px;
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

ul {
    margin: auto;
    width: 800px;
    list-style-type: none;
    font-family: 'Comfortaa';
}

ul li a {
    text-decoration: none;
}

h2 {
    color: #DB2B39;
    font-weight: 700;
    font-size: 1.5em;
    margin-bottom: 13px;
}

#by {
    font-family: 'Zen Maru Gothic';
    font-weight: 800;
    margin-top: 0px;
    font-size: 1.2em;
    margin-bottom: 35px;
}

p {
    font-weight: 500;
}

#content {
    margin-bottom: 50px;
}
</style>
