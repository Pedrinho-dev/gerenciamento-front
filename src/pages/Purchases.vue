<template>
  <v-container>
    <v-row>
      <!-- Card do produto 1 -->
      <v-col cols="6">
        <v-card>
          <v-card-title>Product</v-card-title>
          <v-card-subtitle>Price: R$ 100,00</v-card-subtitle>
          <v-card-subtitle>Quantity: 50</v-card-subtitle>
          <v-card-actions>
            <v-btn color="primary" @click="dialog = true">Buy</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <!-- Card do produto 2 -->
      <v-col cols="6">
        <v-card>
          <v-card-title>Product</v-card-title>
          <v-card-subtitle>Price: R$ 100,00</v-card-subtitle>
          <v-card-subtitle>Quantity: 50</v-card-subtitle>
          <v-card-actions>
            <v-btn color="primary" @click="dialog = true">Buy</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <!-- Modal de compra -->
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>Buy Product</v-card-title>
        <v-card-text>
          <v-form>
            <v-select label="Select"
              :items="['credit']">
            </v-select>
            <v-text-field label="Quantity" v-model="numeroProdutos" type="number"></v-text-field>
            <v-select label="Select"
              :items="['debit', 'credit']">
            </v-select>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn color="blue darken-1" @click="dialog = false">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="confirmarCompra">Confirm</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script setup>
import { ref } from 'vue';

const dialog = ref(false);
const cliente = ref('');
const numeroProdutos = ref(1);

const confirmarCompra = () => {
  // Lógica para confirmar a compra
  console.log(`Cliente: ${cliente.value}, Número de Produtos: ${numeroProdutos.value}`);
  dialog.value = false;
};
</script>

<style>
/* Adicione estilos personalizados aqui, se necessário */
</style>