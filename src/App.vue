<script setup lang="ts">
import { ref, onMounted, watch } from "vue";
import GuitarCard from "./components/GuitarCard.vue";
import HeaderComponent from "./components/HeaderComponent.vue";
import FooterComponent from "./components/FooterComponent.vue";
import { type Guitar, db } from "./data/guitars";
import type { CartItem } from "./data/cartItem";
import Swal from "sweetalert2";

const guitars = ref<Guitar[]>([] as Guitar[]);
const headerGuitar = ref<Guitar>({} as Guitar);
const cart = ref<CartItem[]>([] as CartItem[]);

watch(
  cart,
  () => {
    saveLocalStorage();
  },
  { deep: true },
);

onMounted(() => {
  guitars.value = db;
  headerGuitar.value = guitars.value[3];

  const cartFromLocalStorage = localStorage.getItem("cart");
  if (cartFromLocalStorage) {
    cart.value = JSON.parse(cartFromLocalStorage);
  }
});

const saveLocalStorage = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value));
};

const addToCart = (guitar: Guitar) => {
  const guitarInCartIndex = cart.value.findIndex((cart) => cart.guitar.id === guitar.id);
  if (guitarInCartIndex === -1) {
    cart.value.push({ guitar, quantity: 1 });
  } else {
    cart.value[guitarInCartIndex].quantity += 1;
  }

  Swal.fire({
    title: "Producto agregado al carrito",
    icon: "success",
    showConfirmButton: false,
    toast: true,
    position: "top-right",
    timer: 1500,
  });
};

const decreaseQuantity = (id: number) => {
  const guitarInCartIndex = cart.value.findIndex((cart) => cart.guitar.id === id);
  if (cart.value[guitarInCartIndex].quantity === 1) {
    cart.value = cart.value.filter((cart) => cart.guitar.id !== id);
  } else {
    cart.value[guitarInCartIndex].quantity -= 1;
  }
};

const increaseQuantity = (id: number) => {
  const guitarInCartIndex = cart.value.findIndex((cart) => cart.guitar.id === id);
  if (cart.value[guitarInCartIndex].quantity >= 99) {
    return;
  }
  cart.value[guitarInCartIndex].quantity += 1;
};

const removeFromCart = (id: number) => {
  Swal.fire({
    title: "¿Quieres eliminar este producto?",
    icon: "question",
    showCancelButton: true,
    cancelButtonText: "Cancelar",
    confirmButtonText: "Eliminar",
    confirmButtonColor: "oklch(75% 0.183 55.934)",
  }).then((result) => {
    if (result.isConfirmed) {
      cart.value = cart.value.filter((cart) => cart.guitar.id !== id);
    }
  });
};

const clearCart = () => {
  Swal.fire({
    title: "¿Quieres vaciar el carrito?",
    icon: "question",
    showCancelButton: true,
    cancelButtonText: "Cancelar",
    confirmButtonText: "Vaciar",
    confirmButtonColor: "oklch(75% 0.183 55.934)",
  }).then((result) => {
    if (result.isConfirmed) {
      cart.value = [];
    }
  });
};
</script>

<template>
  <div class="flex flex-col items-center">
    <HeaderComponent
      :header-guitar="headerGuitar!"
      :cart="cart"
      @remove-from-cart="removeFromCart"
      @decrease-quantity="decreaseQuantity"
      @increase-quantity="increaseQuantity"
      @add-to-cart="addToCart"
      @clear-cart="clearCart"
    />

    <main class="max-w-7xl">
      <div class="flex items-center justify-center min-h-[150px]">
        <h2 class="text-4xl md:text-6xl font-black text-orange-400">Nuestra Colección</h2>
      </div>

      <div class="mt-2 grid grid-cols-1 lg:grid-cols-2 xl:grid-cols-3 gap-4">
        <GuitarCard v-for="guitar in guitars" :key="guitar.id" :guitar="guitar" @add-to-cart="addToCart" />
      </div>
    </main>

    <FooterComponent />
  </div>
</template>

<style scoped>
.bg-header {
  background: linear-gradient(to right, rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), url(/img/header.jpg);
  background-size: cover;
  background-position: center;
}

@keyframes guitar-animation {
  0% {
    opacity: 0;
    transform: translateX(-10rem);
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}

.header-guitar {
  animation: guitar-animation 1s ease-in-out;
}
</style>
