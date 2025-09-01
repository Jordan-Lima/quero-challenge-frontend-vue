<script
  lang="ts"
  generic="Card extends { id: string }"
  setup
>
import { defineProps } from 'vue';

defineProps<{
  cards: Card[];
}>();
</script>

<template>
<ul class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 2xl:grid-cols-5 gap-4">
  <li
    v-for="card in cards"
    :key="card.id"
  >
    <slot :card="card" />
  </li>
</ul>
</template>