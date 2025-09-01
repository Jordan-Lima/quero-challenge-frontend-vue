<script
  lang="ts"
  setup
>
import { useAttrs, defineProps, defineEmits } from 'vue';

const props = defineProps<{
  modelValue: string;
}>();

const emit = defineEmits(['update:modelValue']);

const onInput = (event: Event) => {
  emit('update:modelValue', (event.target as HTMLInputElement).value);
};

const attrs = useAttrs();
</script>

<template>
<input
  v-bind="attrs"
  :value="modelValue"
  @input="onInput"
  class="border border-zinc-300 max-w-[80vw] p-3 text-sm rounded-lg"
/>
</template>