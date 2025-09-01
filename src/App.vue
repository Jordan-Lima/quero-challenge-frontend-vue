<script
  setup
  lang="ts"
>
import { ref, onMounted, computed } from 'vue';

import QHeader from "./components/QHeader.vue";
import QInput from "./components/QInput.vue";
import QButton from "./components/QButton.vue";
import QCardOffer from "./components/QCardOffer.vue";
import QFooter from "./components/QFooter.vue";
import QLayout from "./components/QLayout.vue";
import QListCard from "./components/QListCard.vue";
import QSectionForm from "./components/QSectionForm.vue";
import QFormOrderByOffer from "./components/QFormOrderByOffer.vue";
import QFormFilterOffer from "./components/QFormFilterOffer.vue";

interface Offer {
  id: string;
  courseName: string;
  rating: number;
  fullPrice: number;
  offeredPrice: number;
  kind: string;
  level: string;
  iesLogo: string;
  iesName: string;
}

const allOffers = ref<Offer[]>([]);
const searchQuery = ref('');
const appliedSearchQuery = ref('');
const sortBy = ref('course-name');
const filters = ref({
  level: [] as string[],
  kind: [] as string[],
  price: 10000,
});

onMounted(async () => {
  try {
    const response = await fetch('http://localhost:3000/offers');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    allOffers.value = data.map((offer: any, index: number) => ({
      ...offer,
      id: `${offer.courseName}-${offer.iesName}-${index}`
    }));
  } catch (error) {
    console.error('There has been a problem with your fetch operation:', error);
  }
});

const filteredOffers = computed(() => {
  return allOffers.value.filter(offer => {
    const levelMatch = filters.value.level.length === 0 || filters.value.level.includes(offer.level);
    const kindMatch = filters.value.kind.length === 0 || filters.value.kind.includes(offer.kind);
    const priceMatch = offer.offeredPrice <= filters.value.price;
    return levelMatch && kindMatch && priceMatch;
  });
});

const applySearch = () => {
  appliedSearchQuery.value = searchQuery.value;
};

const searchedOffers = computed(() => {
  const query = appliedSearchQuery.value.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '');
  if (!query) {
    return filteredOffers.value;
  }
  return filteredOffers.value.filter(offer =>
    offer.courseName.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '').includes(query)
  );
});

const sortedOffers = computed(() => {
  const offersCopy = [...searchedOffers.value];
  switch (sortBy.value) {
    case 'course-name':
      return offersCopy.sort((a, b) => a.courseName.localeCompare(b.courseName));
    case 'price':
      return offersCopy.sort((a, b) => a.offeredPrice - b.offeredPrice);
    case 'rating':
      return offersCopy.sort((a, b) => b.rating - a.rating);
    default:
      return offersCopy;
  }
});
</script>

<template>
<QLayout>
  <template #header>
    <QHeader>
      <QInput
        type="search"
        id="site-search"
        name="q"
        placeholder="Busque o curso ideal para você"
        aria-label="Buscar cursos e bolsas"
        class="flex-1"
        v-model="searchQuery"
      />
      <QButton type="button" @click="applySearch">Buscar</QButton>
    </QHeader>
  </template>

  <template #sidebar>
    <QFormFilterOffer v-model="filters" />
  </template>

  <QSectionForm title="Veja as opções que encontramos">
    <template #filter>
      <QFormFilterOffer v-model="filters" />
    </template>

    <template #order-by>
      <QFormOrderByOffer v-model="sortBy" />
    </template>
  </QSectionForm>

  <QListCard
    :cards="sortedOffers"
    class="mt-6"
  >
    <template #default="{ card }">
      <QCardOffer
        :course-name="card.courseName"
        :rating="card.rating"
        :full-price="card.fullPrice"
        :offered-price="card.offeredPrice"
        
        :kind="card.kind"
        :level="card.level"
        :ies-logo="card.iesLogo"
        :ies-name="card.iesName"
      />
    </template>
  </QListCard>

  <template #footer>
    <QFooter />
  </template>
</QLayout>
</template>