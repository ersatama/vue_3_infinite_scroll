<template>
  <div class="cards">
    <slot></slot>
  </div>
</template>

<script setup lang="ts">

</script>

<style lang="scss">
.cards {
  margin: 50px auto 50px auto;
  width: 500px;
  display: flex;
  flex-direction: column;
  gap: 25px;
}
</style>