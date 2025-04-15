<script setup lang="ts">
defineProps<{
  guitar: {
    id: number;
    name: string;
    description: string;
    price: number;
    image: string;
  };
}>();
const emit = defineEmits(["add-to-cart"]);
</script>

<template>
  <div class="flex w-[400px]">
    <div class="w-full h-auto">
      <img :src="`/img/${guitar.image}.jpg`" :alt="guitar.name" />
    </div>
    <div class="flex flex-col justify-between">
      <h3 class="uppercase text-sm xs:text-2xl font-bold">{{ guitar.name }}</h3>
      <p class="text-sm xs:text-base">{{ guitar.description }}</p>
      <p class="uppercase text-3xl xs:text-5xl font-black text-orange-400">{{ `${guitar.price} €` }}</p>
      <button
        type="button"
        class="bg-zinc-800 hover:bg-zinc-700 active:bg-zinc-800 text-sm xs:text-base text-white uppercase font-black py-2 px-4"
        @click="emit('add-to-cart', guitar)"
      >
        Agregar al Carrito
      </button>
    </div>
  </div>
</template>
