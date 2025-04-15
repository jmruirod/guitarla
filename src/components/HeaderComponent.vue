<script setup lang="ts">
import type { CartItem } from "@/data/cartItem";
import type { PropType } from "vue";
import ShoppingCartItem from "./ShoppingCartItem.vue";
import type { Guitar } from "@/data/guitars";
import { computed } from "vue";

const props = defineProps({
  cart: {
    type: Array as PropType<CartItem[]>,
    required: true,
  },
  headerGuitar: {
    type: Object as PropType<Guitar>,
    required: true,
  },
});

const emit = defineEmits(["remove-from-cart", "decrease-quantity", "increase-quantity", "add-to-cart", "clear-cart"]);

const removeFromCart = (id: number) => {
  emit("remove-from-cart", id);
};

const decreaseQuantity = (id: number) => {
  emit("decrease-quantity", id);
};

const increaseQuantity = (id: number) => {
  emit("increase-quantity", id);
};

const totalPrice = computed(() => {
  return props.cart.reduce((total, cartItem) => total + cartItem.guitar.price * cartItem.quantity, 0);
});
</script>

<template>
  <header class="relative py-10 w-full flex justify-center bg-header">
    <div class="w-full max-w-[95%] lg:max-w-[80%] flex flex-col justify-between min-h-[75vh]">
      <div class="flex flex-col lg:flex-row justify-start items-center lg:justify-between lg:items-end gap-y-8">
        <div class="w-[80%] lg:w-[25%]">
          <a href="index.html">
            <img class="max-w-full h-auto" src="/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav class="self-end flex items-start justify-end">
          <div class="relative max-w-[2rem] hover:[&>div]:flex z-1">
            <img class="max-w-full h-auto" src="/img/carrito.png" alt="imagen carrito" />

            <div class="p-4 hidden hover:block absolute top-4 -right-3.5">
              <div class="bg-white p-3 w-md flex flex-col gap-2 shadow-lg">
                <p v-if="cart.length === 0" class="text-center py-2 text-zinc-600">El carrito esta vacio</p>
                <div v-else>
                  <div class="grid grid-cols-[repeat(minmax(0,2fr))_minmax(0,1fr)] justify-items-center items-start">
                    <div class="font-bold">Imagen</div>
                    <div class="font-bold">Nombre</div>
                    <div class="font-bold">Precio</div>
                    <div class="font-bold">Cantidad</div>
                    <div class="font-bold"></div>

                    <div class="col-span-5 h-0.5 w-full bg-zinc-200 my-2"></div>

                    <ShoppingCartItem
                      v-for="cartItem in cart"
                      :key="cartItem.guitar.id"
                      :cartItem="cartItem"
                      @remove-from-cart="removeFromCart"
                      @decrease-quantity="decreaseQuantity"
                      @increase-quantity="increaseQuantity"
                    />
                  </div>
                  <p class="text-end">
                    Total pagar: <span class="font-bold">{{ totalPrice }}€</span>
                  </p>
                  <button
                    class="mt-5 w-full bg-zinc-800 hover:bg-zinc-700 active:bg-zinc-800 text-white uppercase font-black py-2 px-4"
                    @click="emit('clear-cart')"
                  >
                    Vaciar Carrito
                  </button>
                </div>
              </div>
            </div>
          </div>
        </nav>
      </div>

      <div class="flex flex-col gap-y-10 w-full lg:w-[50%] items-center lg:items-start">
        <h1 class="text-5xl lg:text-7xl font-bold text-orange-400">Modelo {{ headerGuitar.name }}</h1>
        <p class="mt-5 text-white text-center lg:text-left text-xl">
          {{ headerGuitar.description }}
        </p>
        <div class="flex flex-col items-center lg:items-start gap-y-3">
          <p class="mt-5 text-7xl font-bold text-orange-400">{{ headerGuitar.price }}€</p>
          <button
            type="button"
            class="mt-5 text-2xl bg-orange-400 hover:bg-orange-300 active:bg-orange-400 text-white font-black py-2 px-5"
            @click="emit('add-to-cart', headerGuitar)"
          >
            Agregar al Carrito
          </button>
        </div>
      </div>
    </div>

    <img class="header-guitar hidden xl:block absolute bottom-0 right-0" src="/img/header_guitarra.png" alt="imagen header" />
  </header>
</template>
