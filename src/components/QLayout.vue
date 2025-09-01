<script
  setup
  lang="ts"
>
</script>

<template>
<div class="min-h-screen flex flex-col">
  <div class="border-b">
    <header class="max-w-screen-2xl mx-auto w-full">
      <slot name="header"></slot>
    </header>
  </div>

  <div class="flex flex-1 max-w-screen-2xl mx-auto w-full">
    <aside class="w-[220px] border-r p-6 hidden lg:block">
      <slot name="sidebar"></slot>
    </aside>

    <main class="flex-1 p-6">
      <slot></slot>
    </main>
  </div>

  <footer class="border-t">
    <slot name="footer"></slot>
  </footer>
</div>
</template>