<template>
  <div class="brand-filter">
    <h4>Brands</h4>
    <div v-for="option in options" :key="option">
      <input
        type="checkbox"
        :id="option"
        :value="option"
        v-model="selectedBrands"
        @change="handleBrandChange"
      />
      <label :for="option">{{ option }}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BrandFilter',
  props: {
    options: { type: Array, required: true },
  },
  data() {
    return {
      selectedBrands: [],
    };
  },
  methods: {
    handleBrandChange() {
      this.$emit('update:selectedBrands', this.selectedBrands);
    },
  },
};
</script>
