<script
  lang="ts"
  setup
>
import { defineProps, defineEmits } from 'vue';
import QText from "./QText.vue";

defineProps<{
  label: string;
  checked: boolean;
}>();

const emit = defineEmits(['update:checked']);

const onchange = (event: Event) => {
  emit('update:checked', (event.target as HTMLInputElement).checked);
};
</script>

<template>
<label class="flex items-center text-sm py-2 justify-items-start cursor-pointer">
  <input
    type="checkbox"
    :checked="checked"
    @change="onchange"
  />
  <QText
    tag="span"
    weight="light"
    class="ml-2"
  >
    {{ label }}
  </QText>
</label>
</template>