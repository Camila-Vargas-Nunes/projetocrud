<template>
  <div>
      <table class="table">
          <thead>
            <!--menu 1 -->
            <tr>
              <th scope="col">#</th>
              <th scope="col">Nome</th>
              <th scope="col">Descrição</th>
              <th scope="col">Valor</th>
              <th scope="col">Selecionar</th>
            </tr>
          </thead>
          <tbody>
              <tr v-for="product in products" :key="product.id">
                  <td>{{ product.id }}</td>
                  <td>{{ product.name }}</td>
                  <td>{{ product.description }}</td>
                  <td>{{ product.price }}</td>
                  <td>
                    <div class="row gap-3">
                      <!-- botão de ação -->
                      <router-link :to="`/products/${product.id}`" class="p-2 col border btn btn-primary">Visualizar</router-link>
                      <router-link :to="`/products/${product.id}/edit`" class="p-2 col border btn btn-success">Editar</router-link>
                      <button @click="deleteProduct(product.id)" type="button" class="p-2 col border btn btn-danger">Deletar</button>
                    </div>
                  </td>
              </tr>
          </tbody>
      </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
data() {
  return {
    products: []
  }
},
async created() {
  try {
    const response = await axios.get('/api/products');
    this.products = response.data;
  } catch (error) {
    console.error(error);
  }
},
methods: {
  async deleteProduct(id) {
    try {
      await axios.delete(`/api/products/${id}`);
      this.products = this.products.filter(product => product.id !== id);
    } catch (error) {
      console.error(error);
    }
  }
}
}
</script>