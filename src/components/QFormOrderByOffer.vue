<script
  setup
  lang="ts"
>
import { defineProps, defineEmits } from 'vue';
import QInputRadio from "./QInputRadio.vue";
import QHeading from "./QHeading.vue";

defineProps<{
  modelValue: string;
}>();

const emit = defineEmits(['update:modelValue']);

const onchange = (event: Event) => {
  emit('update:modelValue', (event.target as HTMLInputElement).value);
};

</script>

<template>
<form action="#">
  <QHeading
    tag="h2"
    size="sm"
    class="mb-2"
  >
    Ordenar
  </QHeading>

  <QInputRadio
    label="Cursos de A-Z"
    name="order-by"
    value="course-name"
    :checked="modelValue === 'course-name'"
    @change="onchange"
  />

  <QInputRadio
    label="Menor preço"
    name="order-by"
    value="price"
    :checked="modelValue === 'price'"
    @change="onchange"
  />

  <QInputRadio
    label="Melhor avaliados"
    name="order-by"
    value="rating"
    :checked="modelValue === 'rating'"
    @change="onchange"
  />
</form>
</template>
