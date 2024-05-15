<script setup>
import { Icon } from "@iconify/vue";

// eslint-disable-next-line
const props = defineProps({
  idProduct: Number,
  dataProduct: Object,
  isMan: Boolean,
  isWoman: Boolean,
  isOther: Boolean,
});

// eslint-disable-next-line
defineEmits(["nextProduct"]);
</script>

<template>
  <div
    class="bg"
    :class="{ 'bg-wm': isWoman, 'bg-mn': isMan, 'bg-ot': isOther }"
    style="position: absolute; top: 0; right: 0"
  ></div>
  <div class="card-product m-auto">
    <div v-if="!isOther" class="card-product-inner flex">
      <div class="image-product">
        <img
          :src="props.dataProduct.image"
          alt=""
          class="img-product"
          width="300px"
        />
      </div>
      <div>
        <h1 class="title" :class="{ 'text-wm': isWoman, 'text-mn': isMan }">
          {{ props.dataProduct.title }}
        </h1>
        <div class="category-product flex justify-between border-bottom">
          <p class="text-secondary" style="align-self: center">
            {{ props.dataProduct.category }}
          </p>
          <div class="flex">
            <span
              class="text-secondary"
              style="align-self: center; padding-right: 12px"
              >{{ props.dataProduct.rating.rate }} / 5</span
            >
            <span>
              <Icon
                v-for="i in Math.round(props.dataProduct.rating.rate)"
                :key="i"
                icon="carbon:circle-solid"
                width="24"
                :class="{ 'text-wm': isWoman, 'text-mn': isMan }"
                height="24"
              />
              <Icon
                v-for="i in 5 - Math.round(props.dataProduct.rating.rate)"
                :key="i"
                icon="carbon:circle-outline"
                width="24"
                height="24"
              />
            </span>
          </div>
        </div>
        <div class="detail-product">
          <p class="description border-bottom">
            {{ props.dataProduct.description }}
          </p>
          <p
            class="price-product"
            :class="{ 'text-wm': isWoman, 'text-mn': isMan }"
          >
            ${{ props.dataProduct.price }}
          </p>
        </div>
        <div class="flex justify-between" style="gap: 0 20px">
          <button
            class="button-primary"
            :class="{
              'bg-button-wm border-wm': isWoman,
              'bg-button-mn border-mn': isMan,
            }"
          >
            Buy Now
          </button>
          <button
            @click="$emit('nextProduct')"
            class="button-secondary"
            :class="{
              'text-wm border-wm': isWoman,
              'text-mn border-mn': isMan,
            }"
          >
            Next Product
          </button>
        </div>
      </div>
    </div>
    <div class="product-unavailable" v-else>
      <div>
        <img src="/sad-face.svg" alt="" />
      </div>
      <div class="auto-center">
        <p class="text-unavailable">This product is unavailable to show</p>
        <div class="">
          <button
            @click="$emit('nextProduct')"
            class="button-secondary w-full border-ot text-ot"
            :class="{
              'text-wm border-wm': isWoman,
              'text-mn border-mn': isMan,
            }"
          >
            Next Product
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.text-unavailable {
  padding-bottom: 16px;
}
.product-unavailable {
  text-align: center;
  position: relative;
}
.img-product {
  max-height: 400px;
  margin: auto;
}
.button-primary {
  border: 2px solid;
  color: white;
  text-align: center;
  padding: 8px 80px;
  border-radius: 4px;
  width: 50%;
  font-weight: bold;
  cursor: pointer;
}
.button-secondary {
  background-color: white;
  border: 3px solid;
  text-align: center;
  padding: 8px 80px;
  border-radius: 4px;
  width: 50%;
  font-weight: bold;
  cursor: pointer;
}
.text-wm {
  color: var(--main-product-wm-color);
}
.text-mn {
  color: var(--main-product-mn-color);
}
.text-ot {
  color: var(--main-product-ot-color);
}
.border-wm {
  border-color: var(--main-product-wm-color);
}
.border-ot {
  border-color: var(--main-product-ot-color);
}
.border-mn {
  border-color: var(--main-product-mn-color);
}
.bg-button-wm {
  background-color: var(--main-product-wm-color);
}
.bg-button-mn {
  background-color: var(--main-product-mn-color);
}
.bg-wm {
  background-color: var(--bg-wm-color);
}
.bg-mn {
  background-color: var(--bg-mn-color);
}
.bg-ot {
  background-color: var(--bg-ot-color);
}
.price-product {
  font-size: 28px;
  font-weight: bold;
  text-align: left;
  margin-top: 16px;
  margin-bottom: 16px;
}
.description {
  text-align: left;
  padding-bottom: 64px;
  min-height: 120px;
}
.category-product {
  padding-bottom: 12px;
}
.detail-product {
  margin-top: 24px;
}
.border-bottom {
  border-bottom: #eeeeee 2px solid;
}
.image-product {
  margin-right: 64px;
}
.text-secondary {
  color: var(--text-secondary-color);
}
.title {
  text-align: left;
  font-size: 28px;
  font-weight: 600;
  padding-bottom: 24px;
}
.card-product {
  background-color: white;
  border-radius: 16px;
  max-width: 1024px;
  padding: 72px 56px 64px;
  box-shadow: 2px 4px 21px #00000033;
  margin-top: 96px !important;
}
.bg {
  min-height: 60vh;
  width: 100%;
  z-index: -10;
}
.justify-between {
  justify-content: space-between;
}
.justify-center {
  justify-content: center;
}
.flex {
  display: flex;
}
.mx-auto {
  margin-left: auto;
  margin-right: auto;
}
.m-auto {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
}
</style>
