<template>
  <div class="app" ref="app-container">
    <div class="filters">
      <h3>Filters ({{ productCount }} showing)</h3>
      <div class="filter-container">
        <BrandFilter
          :options="options"
          @update:selectedBrands="handleSelectedBrandsUpdate"
          class="brand-filter"
        />
        <div class="filter-modifiers">
          <div class="button-container">
            <button @click="toggleAvailability">
              {{ onlyAvailable ? 'Show all products' : 'Hide all unavailable' }}
            </button>
            <button @click="sortByRelevance">Sort by relevance</button>
          </div>
          <select v-model="priceOrder" @change="updateSort">
            <option value="true">Price: low to high</option>
            <option value="false">Price: high to low</option>
          </select>
        </div>
      </div>
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
import BrandFilter from './components/BrandFilter.vue';

const options = ['all', ...new Set(products.map((product) => product.brand))];

export default {
  name: 'App',
  components: {
    ProductGridItem,
    BrandFilter,
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

    const handleSelectedBrandsUpdate = (newSelectedBrands) => {
      selectedBrands.value = newSelectedBrands;
    };

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
      handleSelectedBrandsUpdate,
    };
  },
};
</script>

<style>
.app {
  display: flex;
  flex-direction: column;
  max-width: 1024px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #606569;
}

.filters {
  position: sticky;
  top: 0;
  background: #f4f4f4;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  z-index: 2;
}

.filter-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.brand-filter {
  border: solid;
  border-color: #ddebe8;
  padding: 10px;
  border-radius: 5px;
}

.product-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  margin-top: 20px;
  z-index: 1;
}

.product-grid-item {
  margin-right: 20px;
  margin-bottom: 20px;
  flex-basis: calc(33.33% - 20px);
  box-sizing: border-box;
}

button {
  background-color: #ddebe8;
  color: #606569;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-top: 10px;
  width: 100%;
  height: 40px;
}

button:hover {
  background-color: #2e675b;
}

select {
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
  width: 100%;
  height: 40px;
  color: #606569;
}
</style>
