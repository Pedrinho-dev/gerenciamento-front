<template>
  <v-container>
    <v-row>
      <!-- Card 1: Número de clientes cadastrados -->
      <v-col cols="4">
        <v-card class="pa-4">
          <v-card-title class="d-flex justify-center">
            <v-progress-circular
              :value="totalClients"
              :size="100"
              :width="15"
              color="primary"
            >
              {{ totalClients }}
            </v-progress-circular>
          </v-card-title>
          <v-card-subtitle class="text-center">Clients</v-card-subtitle>
        </v-card>
      </v-col>
      <!-- Card 2: Número de compras cadastradas -->
      <v-col cols="4">
        <v-card class="pa-4">
          <v-card-title class="d-flex justify-center">
            <v-progress-circular
              :value="totalPurchases"
              :size="100"
              :width="15"
              color="secondary"
            >
              {{ totalPurchases }}
            </v-progress-circular>
          </v-card-title>
          <v-card-subtitle class="text-center">Purchases</v-card-subtitle>
        </v-card>
      </v-col>
      <!-- Card 3: Total de vendas por cliente -->
      <v-col cols="4">
        <v-card class="pa-4">
          <v-card-title class="d-flex justify-center">
            <v-progress-circular
              :value="totalSalesPerClient"
              :size="100"
              :width="15"
              color="success"
            >
              {{ totalSalesPerClient }}
            </v-progress-circular>
          </v-card-title>
          <v-card-subtitle class="text-center">Total Sales per Client</v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

// Variáveis para armazenar os dados
const totalClients = ref(0); // Inicialmente 0
const totalPurchases = ref(200); // Exemplo de valor
const totalSalesPerClient = ref(300); // Exemplo de valor

// Função para buscar os dados
const fetchData = async () => {
  try {
    // Buscar o total de clientes
    const response = await axios.get('http://localhost:3001/clientes/total');
    totalClients.value = response.data.count;
  } catch (error) {
    console.error('Erro ao buscar total de clientes:', error);
  }

  // Aqui você pode adicionar outras chamadas para APIs ou outras fontes de dados
  totalPurchases.value = 200; // Exemplo de valor
  totalSalesPerClient.value = 300; // Exemplo de valor
};

// Carregar os dados quando o componente for montado
onMounted(fetchData);
</script>

<style>
/* Adicione estilos personalizados aqui, se necessário */
</style>