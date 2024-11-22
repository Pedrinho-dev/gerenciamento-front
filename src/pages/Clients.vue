<template>
    <v-container>
        <!-- criando um botão para abrir o modal -->
        <v-row>
            <v-col class="d-flex justify-start align-center">
                <v-text-field style="width: 80px;" label="Search" v-model="searchQuery"></v-text-field>
            </v-col>
            <v-col>
                <v-btn color="primary" @click="searchClients">Search</v-btn>
            </v-col>
            <v-col class="d-flex justify-end">
                <v-btn color="primary" @click="openDialog">
                    Add
                </v-btn>
            </v-col>
        </v-row>
        <!-- criar a tabela de clientes -->
        <v-row>
            <v-col>
                <div class="table-container">
                    <v-table>
                        <thead>
                            <tr>
                                <th>Nome</th>
                                <th>Telefone</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody>
                            <!-- criando for para listar os clientes -->
                            <tr v-for="client in clients" :key="client.id">
                                <td>{{ client.nome }}</td>
                                <td>{{ client.telefone }}</td>
                                <!-- criando botões de delete e edit -->
                                <td>
                                    <v-btn icon @click="editClient(client)">
                                        <v-icon>mdi-pencil</v-icon>
                                    </v-btn>
                                    <v-btn icon @click="deleteClient(client.id)">
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
                    {{ isEdit ? 'Editar Cliente' : 'Adicionar Cliente' }}
                </v-card-title>
                <!-- criando inputs de texto para o modal -->
                <v-card-text>
                    <v-form>
                        <v-text-field label="Name" v-model="clientName"></v-text-field>
                        <v-text-field label="Telephone" v-model="clientTelephone"></v-text-field>
                    </v-form>
                </v-card-text>
                <!-- criando botoes de ação do modal -->
                <v-card-actions>
                    <v-btn color="blue darken-1" @click="dialog = false">Cancelar</v-btn>
                    <v-btn color="blue darken-1" text @click="saveClient">Salvar</v-btn>
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
const clients = ref([]);
const clientName = ref("");
const clientTelephone = ref("");
const isEdit = ref(false);
const currentClientId = ref(null);
const searchQuery = ref("");

// função para listagem dos clientes
const getClients = async () => {
    try {
        const response = await axios.get("http://localhost:3001/clientes");
        clients.value = response.data;
    } catch (error) {
        console.error("Erro ao buscar clientes:", error);
    }
};

// função para deleção dos clientes
const deleteClient = async (id) => {
    try {
        await axios.delete(`http://localhost:3001/clientes/${id}`);
        getClients();
    } catch (error) {
        console.error("Erro ao deletar cliente:", error);
    }
};

// função para salvar e editar os clientes
const saveClient = async () => {
    try {
        if (isEdit.value) {
            await axios.put(`http://localhost:3001/clientes/${currentClientId.value}`, {
                nome: clientName.value,
                telefone: clientTelephone.value,
            });
        } else {
            await axios.post("http://localhost:3001/clientes", {
                nome: clientName.value,
                telefone: clientTelephone.value,
            });
        }
        dialog.value = false;
        getClients();
    } catch (error) {
        console.error("Erro ao salvar cliente:", error);
    }
};

// função para abrir o modal de edição com os dados do cliente
const editClient = (client) => {
    clientName.value = client.nome;
    clientTelephone.value = client.telefone;
    currentClientId.value = client.id;
    isEdit.value = true;
    dialog.value = true;
};

// função para abrir o modal de cadastro
const openDialog = () => {
    clientName.value = "";
    clientTelephone.value = "";
    isEdit.value = false;
    dialog.value = true;
};

// função para buscar clientes por nome
const searchClients = async () => {
    try {
        const response = await axios.get(`http://localhost:3001/clientes/nome/${searchQuery.value}`);
        clients.value = response.data;
    } catch (error) {
        console.error("Erro ao buscar clientes:", error);
    }
};

// aparecer dados na tela assim que a página for carregada
onMounted(getClients);
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