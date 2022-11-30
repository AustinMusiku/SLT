<template>
  <nuxt-link :to="`learning/${letter}`" class="heading letter">
    {{ letter }}
  </nuxt-link>
</template>

<script setup>
// eslint-disable-next-line no-undef
defineProps({
  letter: {
    type: String,
    required: true
  }
})
</script>

<style lang="scss">
.letter{
    padding: .5em;
    display: grid;
    place-items: center;
    line-height: .8;
    color: $clr-light;
    border: 1px solid #ffffff45;
    border-radius: 5px;
    transition: .25s;
    font-family: $ff-heading;

    &:hover{
        color: $clr-blue;
        background-color: $clr-yellow;
    }
}

.caption{
    margin: 2em 0 0;
    width: 100%;
    text-align: center;
}
</style>
