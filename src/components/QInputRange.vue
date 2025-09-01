<script
  lang="ts"
  setup
>
import { defineProps, useAttrs, defineEmits } from 'vue';
import QText from "./QText.vue";

defineProps<{
  label: string;
  modelValue: number;
}>();

const emit = defineEmits(['update:modelValue']);

const oninput = (event: Event) => {
  emit('update:modelValue', Number((event.target as HTMLInputElement).value));
};

const attrs = useAttrs();
</script>

<template>
<QText
  tag="label"
  weight="light"
>
  {{ label }}
</QText>
<input
  v-bind="attrs"
  type="range"
  :value="modelValue"
  @input="oninput"
  class="w-full mt-3 cursor-col-resize"
/>
</template>