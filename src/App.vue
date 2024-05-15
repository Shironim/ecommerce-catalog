<template>
  <div v-if="loading" class="w-full h-screen" style="position: relative;">
    <div class="loader auto-center"></div>
  </div>

  <ProductSection
    v-else
    :idProduct="idProduct"
    @nextProduct="nextProduct"
    :dataProduct="data"
    :isMan="isMan"
    :isWoman="isWoman"
    :isOther="isOther"
  />
</template>

<script setup>
import "./assets/css/reset.css";
import { ref, watch } from "vue";
import ProductSection from "./components/ProductSection.vue";
const idProduct = ref(1);
const loading = ref(true);
const data = ref(null);
const error = ref(null);
const isMan = ref(false);
const isWoman = ref(false);
const isOther = ref(false);

async function fetchData(id) {
  loading.value = true;
  isMan.value = isWoman.value = isOther.value = false;
  try {
    data.value = await getData(id);
    if (data.value.category == "men's clothing") {
      isMan.value = true;
    } else if (data.value.category == "women's clothing") {
      isWoman.value = true;
    } else {
      isOther.value = true;
    }
  } catch (err) {
    error.value = err.toString();
  } finally {
    loading.value = false;
  }
}

const getData = async (id) => {
  try {
    const res = await fetch(`https://fakestoreapi.com/products/${id}`);
    if (!res.ok) {
      throw new Error(`Error: ${res.status}`);
    }
    const json = await res.json();
    return json;
  } catch (err) {
    throw new Error(`Fetching data failed: ${err.message}`);
  }
};
const nextProduct = () => {
  if (idProduct.value >= 1 && idProduct.value < 20) {
    return idProduct.value++;
  }
  return (idProduct.value = 1);
};

watch(() => idProduct.value, fetchData, { immediate: true });
</script>

<style>
.w-full {
  width: 100% !important;
}
.h-screen {
  height: 100vh !important;
}
#app {
  --main-product-wm-color: #720060;
  --main-product-mn-color: #002772;
  --main-product-ot-color: #3f3f3f;
  --text-primary-color: #1e1e1e;
  --text-secondary-color: #3f3f3f;
  --bg-wm-color: #fde2ff;
  --bg-mn-color: #d6e6ff;
  --bg-ot-color: #dcdcdc;
  font-family: "Inter", sans-serif;
  font-optical-sizing: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  height: 100%;
}
.loader {
  border: 16px solid #f3f3f3; /* Light grey */
  border-top: 16px solid #3498db; /* Blue */
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
}
.auto-center {
  position: absolute;
  top: 50%; /* position the top  edge of the element at the middle of the parent */
  left: 50%; /* position the left edge of the element at the middle of the parent */
  transform: translate(-50%, -50%);
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
