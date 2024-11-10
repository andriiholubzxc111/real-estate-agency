<template>
  <div class="product-list-container">
    <div class="product-list-row" v-for="product in products" :key="product.id">
      <ProductItem :product="product" @productDeleted="removeProduct"/>
    </div>
  </div>
</template>

<script>
import ProductItem from './ProductItem.vue';
import DbOperations from '@/store/helpers/DbOperations';

const productDb = new DbOperations('productData');

export default {
  name: 'ProductList',
  components: {
    ProductItem
  },
  data() {
    return {
      products: [],
      loading: false,
      error: null
    };
  },
  created() {
    this.loadProducts();
  },
  methods: {
    loadProducts() {
      this.loading = true;
      this.error = null;
      productDb.loadItemsList()
        .then(products => {
          this.products = products;
        })
        .catch(error => {
          this.error = error;
        })
        .finally(() => {
          this.loading = false;
        });
    },
    removeProduct(productId) {
      this.products = this.products.filter(product => product.id !== productId);
    }
  }
}
</script>

<style scoped>
.product-list-container {
  display: flex;
  flex-wrap: wrap;
  padding: 16px;
}

.product-list-row {
  display: flex;
  flex: 1 1 calc(33.333% - 16px); /* Визначаємо ширину картки */
  margin: 8px; /* Відступ між картками */
  box-sizing: border-box; /* Для врахування відступів в ширині */
}
</style>