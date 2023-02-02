<script setup lang="ts">
import GeneratorMain from './components/GeneratorMain.vue';
</script>

<template>
  <GeneratorMain />
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@700&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
}

body {
  background: $very-dark-grey;
  margin: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: 'JetBrains Mono';
  font-weight: 700;
}

h1 {
  font-size: 2rem;
  line-height: 42px;
}

h2 {
  font-size: 1.5rem;
  line-height: 32px;
}

h3 {
  font-size: 1.125rem;
  line-height: 24px;
}

button {
  font-family: inherit;
}

@media (max-width: 35rem) {
  h1 {
    font-size: 1.5rem;
    line-height: 32px;
  }

  h2 {
    font-size: 1.125rem;
    line-height: 24px;
  }

  h3 {
    font-size: 1rem;
    line-height: 21px;
  }
}
</style>
