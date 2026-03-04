<template>
  <nav>
    <router-link to="/">Home</router-link>
    |
    <router-link to="/history">History</router-link>
  </nav>
  <router-view />
</template>

<style>
nav {
  padding: 15px;
  text-align: center;
  background: #f5f5f5;
}

a {
  margin: 0 10px;
}
</style>