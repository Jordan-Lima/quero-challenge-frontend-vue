<script
  setup
  lang="ts"
>
import { ref } from 'vue';
import QButton from "./QButton.vue";
import QIconSearch from "./QIconSearch.vue";
import QBottomSheet from "./QBottomSheet.vue";

const openSearch = ref(false);
</script>

<template>
<section class="h-20 bg-white lg:flex items-center hidden">
  <div class="w-[220px] p-6 border-r">
    <figure class="flex items-center h-full">
      <img
        src="https://kong.quero.com/logotype-querobolsa.svg"
        alt="Quero Bolsa"
        height="28px"
        width="181px"
      />
    </figure>
  </div>

  <div class="flex-1 p-6">
    <div class="flex gap-4 items-center">
      <slot />
    </div>
  </div>
</section>

<section class="flex lg:hidden h-20 bg-white px-4 lg:px-8">
  <div class="flex justify-between h-full items-center w-full">
    <figure class="flex items-center h-full">
      <img
        src="https://kong.quero.com/logotype-querobolsa.svg"
        alt="Quero Bolsa"
        height="28px"
        width="181px"
      />
    </figure>

    <QButton
      variant="secondary"
      @click="openSearch = true"
    >
      <QIconSearch />
    </QButton>
  </div>
</section>

<QBottomSheet
  :open="openSearch"
  @closed="openSearch = false"
>
  <div class="flex gap-2 flex-col">
    <slot />
  </div>
</QBottomSheet>
</template>
