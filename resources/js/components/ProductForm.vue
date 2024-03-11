<template>
  <div>
    <h2 v-if="isNewProduct">Adicionar Produto</h2>
    <h2 v-else>Editar Produto</h2>
      <form @submit.prevent="submitForm">
        <div class="mb-3">
          <label for="name" class="form-label">Nome:</label>
          <input class="form-control" type="text" id="name" v-model="product.name" required />
        </div>
        <div class="mb-3">
          <label for="description" class="form-label">Descrição:</label>
          <textarea class="form-control" id="description" v-model="product.description" required></textarea>
        </div>
        <div class="mb-3">
          <label for="price" class="form-label">Valor:</label>
          <input class="form-control" type="number" id="price" v-model="product.price" required />
        </div>
        <button type="submit" v-if="isNewProduct" class="btn btn-primary">Adicionar</button>
        <button type="submit" v-else class="btn btn-primary">Salvar</button>
      </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      product: {
        name: '',
        description: '',
        price: 0
      }
    }
  },
  computed: {
    isNewProduct() {
      return !this.$route.path.includes('edit');
    }
  },
  async created() {
    if (!this.isNewProduct) {
      const response = await axios.get(`/api/products/${this.$route.params.id}`);
      this.product = response.data;
    }
  },
  methods: {
    async submitForm() {
      try {
        if (this.isNewProduct) {
          await axios.post('/api/products', this.product);
        } else {
          await axios.put(`/api/products/${this.$route.params.id}`, this.product);
        }
        this.$router.push('/');
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>
