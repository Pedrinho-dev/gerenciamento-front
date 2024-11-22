<template>
    <v-container>
        <!-- criando um botão para abrir o modal -->
        <v-row>
            <v-col class="d-flex justify-end">
                <v-btn color="primary" @click="openDialog">
                    Products CRUD
                </v-btn>
            </v-col>
        </v-row>
        <!-- criar a tabela de produtos -->
        <v-row>
            <v-col>
                <div class="table-container">
                    <v-table>
                        <thead>
                            <tr>
                                <th>Nome</th>
                                <th>Preço</th>
                                <th>Estoque</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody>
                            <!-- criando for para listar os produtos -->
                            <tr v-for="product in products" :key="product.id">
                                <td>{{ product.nome }}</td>
                                <td>{{ product.preco }}</td>
                                <td>{{ product.estoque }}</td>
                                <!-- criando botões de delete e edit -->
                                <td>
                                    <v-btn icon @click="editProduct(product)">
                                        <v-icon>mdi-pencil</v-icon>
                                    </v-btn>
                                    <v-btn icon @click="deleteProduct(product.id)">
                                        <v-icon>mdi-delete</v-icon>
                                    </v-btn>
                                </td>
                            </tr>
                        </tbody>
                    </v-table>
                </div>
            </v-col>
        </v-row>
        <!-- criando modal -->
        <v-dialog v-model="dialog">
            <v-card>
                <!-- criando titulo do card -->
                <v-card-title>
                    {{ isEdit ? 'Editar Produto' : 'Adicionar Produto' }}
                </v-card-title>
                <!-- criando inputs de texto para o modal -->
                <v-card-text>
                    <v-form>
                        <v-text-field label="Name" v-model="productName"></v-text-field>
                        <v-text-field label="Price" v-model="productPrice"></v-text-field>
                        <v-text-field label="Quantity" v-model="productQuantity"></v-text-field>
                    </v-form>
                </v-card-text>
                <!-- criando botoes de ação do modal -->
                <v-card-actions>
                    <v-btn color="blue darken-1" @click="dialog = false">Cancelar</v-btn>
                    <v-btn color="blue darken-1" text @click="saveProduct">Salvar</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-container>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

// definindo os tipos das variáveis
const dialog = ref(false);
const products = ref([]);
const productName = ref("");
const productPrice = ref("");
const productQuantity = ref("");
const isEdit = ref(false);
const currentProductId = ref(null);

// função para listagem dos produtos
const getProducts = async () => {
    try {
        const response = await axios.get("http://localhost:3001/produtos");
        products.value = response.data;
    } catch (error) {
        console.error("Erro ao buscar produtos:", error);
    }
};

// função para deleção dos produtos
const deleteProduct = async (id) => {
    try {
        await axios.delete(`http://localhost:3001/produtos/${id}`);
        getProducts();
    } catch (error) {
        console.error("Erro ao deletar produto:", error);
    }
};

// função para salvar e editar os produtos
const saveProduct = async () => {
    try {
        if (isEdit.value) {
            await axios.put(`http://localhost:3001/produtos/${currentProductId.value}`, {
                nome: productName.value,
                preco: productPrice.value,
                estoque: productQuantity.value,
            });
        } else {
            await axios.post("http://localhost:3001/produtos", {
                nome: productName.value,
                preco: productPrice.value,
                estoque: productQuantity.value,
            });
        }
        dialog.value = false;
        getProducts();
    } catch (error) {
        console.error("Erro ao salvar produto:", error);
    }
};

// função para abrir o modal de edição com os dados do produto
const editProduct = (product) => {
    productName.value = product.nome;
    productPrice.value = product.preco;
    productQuantity.value = product.estoque;
    currentProductId.value = product.id;
    isEdit.value = true;
    dialog.value = true;
};

// função para abrir o modal de cadastro
const openDialog = () => {
    productName.value = "";
    productPrice.value = "";
    productQuantity.value = "";
    isEdit.value = false;
    dialog.value = true;
};

// aparecer dados na tela assim que a página for carregada
onMounted(getProducts);
</script>

<style>
.table-container {
    border: 1px solid #ccc;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    overflow: hidden;
    /* Garante que o conteúdo da tabela respeite o border-radius */
}
</style>