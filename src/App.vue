<template>
  <div class="app" ref="app-container">
    <div class="filters">
      <h3>Filters ({{ productCount }} showing)</h3>
      <div class="brandfilter">
        <h4>Brands</h4>
        <div v-for="option in options" :key="option">
          <input
            type="checkbox"
            :id="option"
            :value="option"
            v-model="selectedBrands"
          />
          <label :for="option">{{ option }}</label>
        </div>
      </div>
      <button @click="toggleAvailability">
        {{ onlyAvailable ? 'Show all products' : 'Hide all unavailable' }}
      </button>
      <select v-model="priceOrder" @change="updateSort">
        <option value="true">Price: low to high</option>
        <option value="false">Price: high to low</option>
      </select>
      <button @click="sortByRelevance">Sort by relevance</button>
    </div>

    <div class="product-grid">
      <div v-for="product in filteredProducts" :key="product.id">
        <ProductGridItem :product="product" />
      </div>
    </div>
  </div>
</template>

<script>
import ProductGridItem from './components/ProductGridItem.vue';
import products from './data/products.json';
import { computed, ref } from 'vue';

const options = ['all', ...new Set(products.map((product) => product.brand))];

export default {
  name: 'App',
  components: {
    ProductGridItem,
  },
  setup() {
    const selectedBrands = ref(['all']);
    const onlyAvailable = ref(false);
    const priceOrder = ref(null);
    const sortRelevance = ref(false);

    const filteredProducts = computed(() => {
      let filtered = products;
      if (selectedBrands.value.includes('all')) {
        filtered = products;
      } else {
        filtered = products.filter((product) =>
          selectedBrands.value.includes(product.brand)
        );
      }
      if (onlyAvailable.value) {
        filtered = filtered.filter((product) => product.isAvailable);
      }
      if (priceOrder.value === true) {
        filtered = filtered.sort((a, b) => a.price - b.price);
      }
      if (priceOrder.value === false) {
        filtered = filtered.sort((a, b) => b.price - a.price);
      }
      if (sortRelevance.value) {
        filtered = filtered.sort((a, b) => {
          if (a.isAvailable === b.isAvailable) {
            return a.rank - b.rank;
          } else {
            return a.isAvailable ? -1 : 1;
          }
        });
      }
      return filtered;
    });

    const productCount = computed(() => filteredProducts.value.length);

    const toggleAvailability = () => {
      onlyAvailable.value = !onlyAvailable.value;
    };

    const updateSort = (event) => {
      priceOrder.value = event.target.value === 'true';
      sortRelevance.value = false;
    };

    const sortByRelevance = () => {
      sortRelevance.value = true;
    };

    return {
      options,
      selectedBrands,
      onlyAvailable,
      priceOrder,
      filteredProducts,
      productCount,
      toggleAvailability,
      updateSort,
      sortByRelevance,
    };
  },
};
</script>

<style>
.app {
  max-width: 1024px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #606569;
}

.link {
  color: #3a7f71;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  grid-gap: 2rem;
  margin: 0 auto;
  max-width: 1200px;
  padding: 1rem;
}

.product-grid-item {
  background: #ddebe8;
  max-width: 150px;
  border-radius: 5px;
  padding: 10px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.filters {
  background: #f4f4f4;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
}
</style>
