<template>
  <div class="card">
    <div class="card__header">
      <div class="card__header__img">
        <img :src="card.img" :alt="card.name">
      </div>
      <div class="card__header__info">
        <div class="card__header__info__title"> {{card.name}} </div>
        <div class="card__header__info__email"> {{card.email}} </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const { card } = defineProps(['card'])
</script>

<style lang="scss">
.card {
  background-color: #424242;
  border-radius: 20px;
  &__header {
    display: flex;
    gap: 25px;
    align-items: center;
    &__img {
      border-radius: 50px;
      overflow: hidden;
      & > img {
        margin: 0;
        padding: 0;
        display: block;
      }
    }
    &__info {
      display: flex;
      flex-direction: column;
      gap: 5px;
      &__title {
        font-size: 20px;
        font-weight: 600;
      }
      &__email {
        font-size: 14px;
      }
    }
  }
}
</style>