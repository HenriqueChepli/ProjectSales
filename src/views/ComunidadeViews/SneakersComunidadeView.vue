<template>
    <div class="page-container">
      <div class="logo-container hidden">
        <h1>Calçados</h1>
      </div>
      <div class="main-content">
        <aside class="filter-container hidden">
          <input type="text" placeholder="Filtrar produtos" v-model="filterText" class="filter-input" />
        </aside>
        <div class="product-list">
          <div class="product-item hidden" v-for="product in filteredProducts" :key="product.id" :product="product">
            <button class="btn-more" @click="visualizar(product.id)">
      <img :src="product.image1" :alt="product.name" class="product-image" />
    </button>
      <div class="buttons">
        <div>
        <h3>{{ product.name }}</h3>
        <p>R${{ product.price }}</p>
        </div>
        <button class="btn-cart" @click="carrinhoStore.addCarrinho(product)">
          <img src="/src/assets/img/Icons/carrinho.svg" alt="Carrinho" class="cart-image" />
        </button>
      </div>
    </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { computed, ref } from 'vue';
  import { useComunidadeStore } from '@/stores/comunidade';
  import { useRouter } from 'vue-router';
  import { useIntersectionObserver } from '@/composables/useIntersectionObserver';
  import { useCarrinhoStore } from '@/stores/carrinho';

const carrinhoStore = useCarrinhoStore();
  useIntersectionObserver
  
  
  const router = useRouter()
  
  function visualizar(id) {
    router.push(`/produtoComunidade/${id}`)
  }
  
  const comunidadeStore = useComunidadeStore();
  
  
  const filterText = ref('')

const filteredProducts = computed(() => 
  comunidadeStore.comunidade.filter(product => 
    product.tenis && 
    (!filterText.value || product.name.toLowerCase().includes(filterText.value.toLowerCase()))
  )
);
  </script>
  
  <style scoped>
  .page-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }
  
  header {
    width: 100%;
  }
  
  .buttons div h3{
    font-size: 14px;
    font-weight: 600;
  }
  
  .buttons div p{
    color: #025213;
    font-weight: 600;
    font-size: 13px;
  }
  
  .logo-container {
    margin: 20px 0;
  }
  
  .nike-logo {
    width: 150px;
    height: auto;
  }
  
  .main-content {
    display: flex;
    flex-direction: column;
    width: 100%;
    align-items: center;
  }
  
  .filter-container {
    width: 100%;
    max-width: 600px;
    padding: 10px;
    box-sizing: border-box;
  }
  
  .filter-input {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    margin-bottom: 20px;
  }
  
  .product-list {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    gap: 20px;
    padding: 20px;
    max-width: 1500px;
  }
  
  .product-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    border: none;
    padding: 20px;
    margin: 15px;
    width: 250px;
    height: auto;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    transition: all .3s ease-in-out;
}

.product-item:hover{
  transform: scale(1.01) translateY(-5px);
}
  
  .product-image {
    max-width: 100%;
    height: auto;
    margin-bottom: 15px;
  }
  
  .buttons {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }
  
  button {
    border: none;
    cursor: pointer;
    font-size: 14px;
  }
  .buttons h3{
    font-size: 14px;
    font-weight: 600;
  }
  
  .btn-more {
    color: black;
    padding: 10px 20px;
    background-color: #ffff;
  }
  
  .btn-cart {
    background: none;
    padding: 0;
  }
  
  .cart-image {
    width: 36px;
    height: 36px;
  }
  </style>
  