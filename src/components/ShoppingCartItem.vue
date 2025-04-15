<script setup lang="ts">
defineProps<{
  cartItem: {
    guitar: {
      id: number;
      name: string;
      description: string;
      price: number;
      image: string;
    };
    quantity: number;
  };
}>();
const emit = defineEmits(["remove-from-cart", "decrease-quantity", "increase-quantity"]);
</script>

<template>
  <div>
    <img class="w-[30px]" :src="`/img/${cartItem.guitar.image}.jpg`" :alt="cartItem.guitar.name" />
  </div>
  <div>{{ cartItem.guitar.name }}</div>
  <div class="font-bold">{{ `${cartItem.guitar.price} €` }}</div>
  <div class="flex justify-center items-center">
    <button
      type="button"
      class="bg-zinc-800 hover:bg-zinc-700 active:bg-zinc-800 text-white font-black min-w-[26px] p-2"
      @click="emit('decrease-quantity', cartItem.guitar.id)"
    >
      -
    </button>
    <span class="w-[24px] text-center">{{ cartItem.quantity }}</span>
    <button
      type="button"
      class="bg-zinc-800 hover:bg-zinc-700 active:bg-zinc-800 text-white font-black min-w-[26px] p-2"
      @click="emit('increase-quantity', cartItem.guitar.id)"
    >
      +
    </button>
  </div>
  <div>
    <button
      @click="emit('remove-from-cart', cartItem.guitar.id)"
      class="bg-red-500 hover:bg-red-600 active:bg-red-500 text-white font-black text-[10px] px-[9px] py-[5px] rounded-[50%]"
      type="button"
    >
      X
    </button>
  </div>

  <div class="col-span-5 h-0.5 w-full bg-zinc-200 my-2"></div>
</template>
