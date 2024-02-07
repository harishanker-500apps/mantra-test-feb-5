<template>
  <div>
    <header class="h-10 bg-cyan-500 w-full p-2 flex justify-evenly sticky top-0 z-10">
      <NuxtLink to="/">Home</NuxtLink>
      <NuxtLink to="/employee">Emloyee</NuxtLink>
      <NuxtLink to="/contact">Contact</NuxtLink>
    </header>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>

<style setup>
body {
  overflow-y: hidden;
}
</style>