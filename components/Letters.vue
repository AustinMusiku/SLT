<template>
  <div class="wrap">
    <div class="letters">
      <Letter v-for="letter in letters" :key="letter" :letter="letter" />
    </div>
  </div>
</template>

<script setup>
import { ref } from '@nuxtjs/composition-api'
const letters = ref(['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y'])
</script>

<style lang="scss">
.wrap{
    .letters{
        margin: 0 7em 0;
        display: grid;
        gap: .5em;
        grid-template-columns: repeat(6, 1fr);
        grid-template-rows: repeat(4, 1fr);
    }
    .caption{
        margin: 2em 0 0;
        width: 100%;
        text-align: center;
    }
}
</style>
