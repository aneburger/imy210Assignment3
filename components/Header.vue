<script setup>
  /* Ane' Burger - 24565068 */
</script>

<template>
    <header>
      <h1>blogger <span>.</span></h1>
      <nav>
        <NuxtLink to="/">Home</NuxtLink> 
        <NuxtLink to="/search">Search</NuxtLink>
      </nav>
    </header>
</template>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@300..700&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic&display=swap');

    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0px;
    }

    nav {
        margin-right: 55px;
        font-family: 'Comfortaa';
    }

    nav a {
        color: #DB2B39;
        text-decoration: none;
        font-size: 1.5em;
        margin-right: 30px;
        font-weight: 700;
        padding: 10px 12px 10px 12px;
        background-color: transparent;
    }

    .router-link-active{
        color: #DB2B39;
        font-weight: 700;
        text-decoration: none;
        margin-right: 50px;
    }

    nav a:hover {
        color: white;
        background-color: #DB2B39;
        border-radius: 30px;
        padding: 10px 12px 10px 12px;
    }

    h1 {
        font-family: 'Comfortaa';
        font-size: 4.5em;
        margin-left: 50px;
        margin-top: 10px;
        font-weight: 700;
    }

    h1 span {
        font-size: 1.7em;
        margin-left: 10px;
        color: #DB2B39;
    }
</style>