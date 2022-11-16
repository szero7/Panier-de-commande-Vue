<template>
  <header>
    <nav class="d-inline-flex mt-2 mt-md-0 ms-md-auto">
      <RouterLink to="/" class="me-3 py-2 px-3">Accueil</RouterLink>
      <RouterLink :to="{ name: 'contact' }" class="me-3 py-2 px-3">Contact</RouterLink>
    </nav>
  </header>
</template>

<style scoped>
a:active, a:hover, a:visited, a:link {
  color: #fff !important;
  text-decoration: none;
}
a.router-link-active,
a.router-link-exact-active {
  color: #fff;
  border-bottom: 5px solid rgb(48, 195, 67);
}

header {
  background-color: #537698;
  padding: 20px;
}
</style>
