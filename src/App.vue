<template>
  <v-app>
    <!-- Criando header da aplicação -->
    <v-app-bar color="primary">
      <v-btn icon @click.stop="drawer = !drawer">
        <v-icon>mdi-menu</v-icon>
      </v-btn>
      <v-app-bar-title>Sistema de gerenciamento</v-app-bar-title>
    </v-app-bar>

    <!-- Criando menu lateral -->
    <v-navigation-drawer v-model="drawer" color="primary">
      <v-list-item to="/home" link prepend-icon="mdi-home-circle-outline">
        Home
      </v-list-item>

      <v-list-item to="/purchases" link prepend-icon="mdi-shopping">Purchases</v-list-item>

      <v-list-item  to="/clients" link prepend-icon="mdi-account-group">Crud Clients</v-list-item>
      <v-list-item  to="/products" link prepend-icon="mdi-format-list-numbered">Crud Products</v-list-item>
    </v-navigation-drawer>

    <!-- Criando conteúdo principal -->
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup>
// ref responsável por mudar o valor dos dados automaticamente
import { ref } from "vue";

// Criando variável para controlar o menu lateral
const drawer = ref(false);

// Função para abrir e fechar o menu lateral
// const toggleDrawer = () => {
//   drawer.value = !drawer.value;
// };
</script>