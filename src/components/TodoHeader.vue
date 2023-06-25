<template>  
    <header>
        <nav class="container">
            <img src="../assets/logo.svg" alt="" height="50" width="50">
            <h1>Vue Todos</h1>
            <ul class="nav-routes">
                <RouterLink to="/">Home</RouterLink>                
                <RouterLink to="/about">About</RouterLink>
            </ul>
        </nav>
    </header>
</template>

<script setup>
import { RouterLink } from 'vue-router';


</script>

<style lang="scss" scoped>

header {
background-color: #f1f1f1;
  nav {
    display: flex;
    align-items: center;
    padding: 25px 25px;
    .nav-routes {
      display: flex;
      flex: 1;
      justify-content: flex-end;
      gap: 20px;
      list-style: none;
        a {
          text-decoration: none;
          color: inherit;
          }
    }
  }
}
</style>