<template>
  <div class="product-grid-item">
    <img class="product-image" :src="product.image" />
    <p class="product-name">{{ product.name }}</p>
    <p>£ {{ product.price }}</p>
  </div>
</template>

<script>
export default {
  name: 'ProductGridItem',
  props: {
    product: { type: Object, required: true, default: () => ({}) },
  },
};
</script>

<style scoped>
.product-grid-item {
  background: #ddebe8;
  max-width: 150px;
}

.product-image {
  width: 100%;
}

p {
  padding-left: 10px;
  padding-bottom: 10px;
}
</style>
