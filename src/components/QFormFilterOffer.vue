<script
  setup
  lang="ts"
>
import { defineProps, defineEmits, computed } from 'vue';
import QHeading from "./QHeading.vue";
import QInputCheckbox from "./QInputCheckbox.vue";
import QInputRange from "./QInputRange.vue";
import QFieldset from "./QFieldset.vue";

const props = defineProps<{
  modelValue: {
    level: string[];
    kind: string[];
    price: number;
  }
}>();

const emit = defineEmits(['update:modelValue']);

const updateLevel = (level: string, checked: boolean) => {
  const newLevels = [...props.modelValue.level];
  if (checked) {
    newLevels.push(level);
  } else {
    const index = newLevels.indexOf(level);
    if (index > -1) {
      newLevels.splice(index, 1);
    }
  }
  emit('update:modelValue', { ...props.modelValue, level: newLevels });
};

const updateKind = (kind: string, checked: boolean) => {
  const newKinds = [...props.modelValue.kind];
  if (checked) {
    newKinds.push(kind);
  } else {
    const index = newKinds.indexOf(kind);
    if (index > -1) {
      newKinds.splice(index, 1);
    }
  }
  emit('update:modelValue', { ...props.modelValue, kind: newKinds });
};

const priceProxy = computed({
  get: () => props.modelValue.price,
  set: (value) => emit('update:modelValue', { ...props.modelValue, price: value })
});

const formatCurrency = (value: number) => {
  return new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
  }).format(value);
};
</script>

<template>
<form action="#">
  <QHeading tag="h1">Filtros</QHeading>

  <hr class="my-5">

  <QFieldset legend="Graduação">
    <QInputCheckbox label="Bacharelado" :checked="modelValue.level.includes('bacharelado')" @update:checked="checked => updateLevel('bacharelado', checked)" />
    <QInputCheckbox label="Licenciatura" :checked="modelValue.level.includes('licenciatura')" @update:checked="checked => updateLevel('licenciatura', checked)" />
    <QInputCheckbox label="Técnologo" :checked="modelValue.level.includes('tecnologo')" @update:checked="checked => updateLevel('tecnologo', checked)" />
  </QFieldset>

  <hr class="my-5">

  <QFieldset legend="Modalidade do curso">
    <QInputCheckbox label="Presencial" :checked="modelValue.kind.includes('presencial')" @update:checked="checked => updateKind('presencial', checked)" />
    <QInputCheckbox label="A distância - EaD" :checked="modelValue.kind.includes('ead')" @update:checked="checked => updateKind('ead', checked)" />
  </QFieldset>

  <hr class="my-5">

  <QFieldset legend="Preço da mensalidade">
    <QInputRange v-model="priceProxy" :label="formatCurrency(priceProxy)" min="0" max="10000" />
  </QFieldset>

  <hr class="mt-5">
</form>
</template>
