<script
  lang="ts"
  setup
>
import { defineProps, computed } from 'vue';
import QIconStar from "./QIconStar.vue";
import QText from "./QText.vue";

const props = defineProps<{
  rating: number;
}>();

const stars = computed(() => {
  const fullStars = Math.floor(props.rating);
  const halfStar = props.rating % 1 >= 0.5 ? 1 : 0;
  const emptyStars = 5 - fullStars - halfStar;
  return [
    ...Array(fullStars).fill('full'),
    ...Array(halfStar).fill('half'),
    ...Array(emptyStars).fill('empty'),
  ];
});

</script>

<template>
<div class="flex items-center gap-2">
  <QText tag="span">
    {{ rating }}
  </QText>
  <div class="flex items-center space-x-1 text-yellow-500">
    <QIconStar v-for="(star, index) in stars" :key="index" :state="star" />
  </div>
</div>
</template>