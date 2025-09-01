<script
  lang="ts"
  setup
>
import { defineProps, computed } from 'vue';
import QHeading from "./QHeading.vue";
import QButton from "./QButton.vue";
import QRating from "./QRating.vue";
import QPrice from "./QPrice.vue";
import QText from "./QText.vue";

const props = defineProps<{
  courseName: string;
  rating: number;
  fullPrice: number;
  offeredPrice: number;
  kind: string;
  level: string;
  iesLogo: string;
  iesName: string;
}>();

const formattedKind = computed(() => {
  if (props.kind === 'presencial') return 'Presencial 🏫';
  if (props.kind === 'ead') return 'EaD 🏠';
  return props.kind;
});

const formattedLevel = computed(() => {
  if (props.level === 'bacharelado') return 'Graduação (bacharelado) 🎓';
  if (props.level === 'tecnologo') return 'Graduação (tecnólogo) 🎓';
  if (props.level === 'licenciatura') return 'Graduação (licenciatura) 🎓';
  return props.level;
});

const formatCurrency = (value: number) => {
  return new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
  }).format(value);
};

const formattedFullPrice = computed(() => formatCurrency(props.fullPrice));
const formattedOfferedPrice = computed(() => formatCurrency(props.offeredPrice));

const discountPercentage = computed(() => {
  const discount = ((props.fullPrice - props.offeredPrice) / props.fullPrice) * 100;
  return `${Math.round(discount)}%`;
});
</script>

<template>

<article class="bg-white p-6 rounded-lg shadow-sm border flex flex-col justify-between items-start gap-3 min-h-[400px]">
  <img
    :src="iesLogo"
    :alt="iesName"
    class="h-10 object-contain"
  />
  <QHeading
    tag="h2"
    size="sm"
  >
    {{ courseName }}
  </QHeading>
  <QRating :rating="rating" />
  <QPrice
    :full-price="formattedFullPrice"
    :offered-price="formattedOfferedPrice"
    :discount="discountPercentage"
  />
  <div>
    <QText tag="p">{{ formattedKind }}</QText>
    <QText
      tag="p"
      color="minor"
      size="sm"
    >
      {{ formattedLevel }}
    </QText>
  </div>
  <QButton
    tag="a"
    size="sm"
    href="#"
    class="w-full"
  >
    Quero esta bolsa
  </QButton>
</article>
</template>