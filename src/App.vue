<template>
  <div id="app">
    <div class="container pt-5">
      <h1>Movy Prototypes</h1>
      <div id="nav" class="mt-1">
        <router-link to="/">Home</router-link> |
        <router-link to="/editor">Editor</router-link>
      </div>
      <router-view/>
    </div>
  </div>
</template>