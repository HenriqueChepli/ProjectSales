<script setup>
import HeaderComponente from './components/BensaComponents/HeaderComponente.vue';
import HeaderComunidadeComponente from './components/ComunidadeComponents/HeaderComunidadeComponente.vue';
import FooterComponente from './components/BensaComponents/FooterComponente.vue';
import { useRoute } from 'vue-router';

const route = useRoute();

</script>

<template>

  <HeaderComponente v-if="route.name !== 'comunidade' && route.name !== 'minhaloja' && route.name !== 'addproduto' && route.name !== 'sneakersComunidade' && route.name !== 'roupasComunidade' && route.name !== 'acessoriosComunidade' && route.name !== 'chat'"/>


  <HeaderComunidadeComponente
    v-if="route.name === 'comunidade' || route.name === 'minhaloja' || route.name === 'addproduto' || route.name === 'sneakersComunidade' || route.name === 'roupasComunidade' || route.name === 'acessoriosComunidade'|| route.name === 'chat'" />

  <router-view></router-view>
  <FooterComponente />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
  font-family: "Poppins", sans-serif;
  padding: 0;
  border: 0;
  margin: 0;
  box-sizing: border-box;
}

a {
  text-decoration: none;
}

html {
	scroll-behavior: smooth;
  overflow-x: hidden;
  }

  body,
html {
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  overflow-y: auto;
}

section {
  background: #ffffff;
  width: 100%;
}

.show{
  transition: all 2s ease;
  opacity: 1;
  transform: translateY(0) translateX(0);
  z-index: 1 !important;
}

.hidden {
  opacity: 0;
  transform: translateY(-50px);
  z-index: 3;
}

.toLeft{
  opacity: 0;
  transform: translateX(50%);
  z-index: 3;
}

.toRight{
  opacity: 0;
  z-index: 3;
  transform: translateX(-50%);
}
</style>
