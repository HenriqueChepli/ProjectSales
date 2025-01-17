<template>
    <div class="product-form">
        <h1 class="form-title">Publicar Produto</h1>

        <div class="input-container toRight">
            <label for="product-title">Título do Produto</label>
            <input type="text" id="product-title" v-model="product.title" placeholder="Ex: Camiseta Estampada"
                required />
        </div>

        <div class="input-container toLeft">
            <label for="brand">Marca</label>
            <input type="text" id="brand" v-model="product.brand" placeholder="Ex: Nike" required />
        </div>

        <div class="input-container toRight">
            <label for="description">Descrição do Produto (até 350 caracteres)</label>
            <textarea id="description" v-model="product.description" maxlength="350"
                placeholder="Descreva o produto aqui..." required></textarea>
        </div>

        <div class="input-container toLeft">
            <label for="category">Categoria</label>
            <select id="category" v-model="product.category" @change="resetSize" required>
                <option value="" disabled>Selecione uma categoria</option>
                <option value="roupas">Roupas</option>
                <option value="calçados">Calçados</option>
                <option value="acessórios">Acessórios</option>
            </select>
        </div>

        <div class="input-container toRight">
            <label for="price">Preço</label>
            <input type="number" id="price" v-model="product.price" placeholder="Ex: 99.99" required />
        </div>

        <div class="input-container toLeft">
            <label for="size">Tamanho</label>
            <select id="size" v-model="product.size" required>
                <option value="" disabled>Selecione um tamanho</option>
                <option v-for="size in sizeOptions" :key="size" :value="size">{{ size }}</option>
            </select>
        </div>

        <div class="image-upload hidden">
            <h2>Adicionar Fotos</h2>
            <div class="image-grid">
                <label class="image-box main-image">
                    <input type="file" accept="image/*" @change="handleImageChange(0)" />
                    <span>+</span>
                </label>
                <label class="image-box">
                    <input type="file" accept="image/*" @change="handleImageChange(1)" />
                    <span>+</span>
                </label>
                <label class="image-box">
                    <input type="file" accept="image/*" @change="handleImageChange(2)" />
                    <span>+</span>
                </label>
                <label class="image-box">
                    <input type="file" accept="image/*" @change="handleImageChange(3)" />
                    <span>+</span>
                </label>
                <label class="image-box">
                    <input type="file" accept="image/*" @change="handleImageChange(4)" />
                    <span>+</span>
                </label>
            </div>
            <p class="image-instructions">Tire fotos de diferentes ângulos do produto, isso vai aumentar suas chances de
                vendê-lo.</p>
        </div>

        <router-link to="/minhaloja">
            <button class="submit-button">Enviar</button>
        </router-link>
    </div>
</template>

<script setup>
import { ref, computed } from "vue"; // Importa as funções ref e computed do Vue para criar dados reativos e propriedades computadas
import { useIntersectionObserver } from '@/composables/useIntersectionObserver'; // Importa o hook de observação de interseção

useIntersectionObserver // Chama o hook para observar a interseção do componente (não está sendo usado diretamente aqui, mas foi importado)

const product = ref({ // Cria um objeto reativo para armazenar os dados do produto
    title: '',
    brand: '',
    description: '',
    category: '',
    price: '',
    size: '',
    images: Array(5).fill(null), // Inicializa um array de imagens com 5 slots vazios
});

const sizeOptions = ref([]); // Cria uma lista reativa para armazenar as opções de tamanho, que serão atualizadas conforme a categoria

// Função para lidar com a mudança de imagem ao selecionar uma foto
const handleImageChange = (index) => {
    const input = event.target; // Acessa o input de imagem
    if (input.files && input.files[0]) { // Verifica se foi selecionado um arquivo
        const reader = new FileReader(); // Cria um leitor de arquivo
        reader.onload = (e) => { // Quando o arquivo for lido
            product.value.images[index] = e.target.result; // Armazena o resultado da leitura (base64 da imagem) no array de imagens
        };
        reader.readAsDataURL(input.files[0]); // Lê o arquivo como uma URL de dados
    }
};

// Função que será chamada ao mudar a categoria, para resetar o tamanho
const resetSize = () => {
    product.value.size = ''; // Reseta o tamanho do produto
    updateSizeOptions(); // Atualiza as opções de tamanho conforme a categoria
};

// Função para atualizar as opções de tamanho com base na categoria do produto
const updateSizeOptions = () => {
    switch (product.value.category) { // Verifica a categoria do produto
        case 'roupas':
            sizeOptions.value = ['P', 'M', 'G', 'GG', 'XGG']; // Opções de tamanho para roupas
            break;
        case 'calçados':
            sizeOptions.value = ['35', '36', '37', '38', '39', '40', '41', '42', '43', '44', '45', '46', '47']; // Opções de tamanho para calçados
            break;
        case 'acessórios':
            sizeOptions.value = ['Tamanho Único']; // Opção única para acessórios
            break;
        default:
            sizeOptions.value = []; // Caso a categoria não seja reconhecida, limpa as opções de tamanho
    }
};
</script>


<style scoped>
.product-form {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
}

.form-title {
    text-align: center;
    margin-bottom: 20px;
}

.input-container {
    margin-bottom: 15px;
}

.input-container label {
    display: block;
    margin-bottom: 5px;
}

input[type="text"],
input[type="number"],
textarea,
select {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

textarea {
    height: 100px;
}

.image-upload {
    margin: 20px 0;
}

.image-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
}

.image-box {
    width: 100%;
    height: 100px;
    background-color: #f0f0f0;
    border: 1px dashed #ccc;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
}

.image-box input {
    position: absolute;
    width: 100%;
    height: 100%;
    opacity: 0;
    cursor: pointer;
}

.main-image {
    grid-column: span 2;
    height: 200px;
}

.image-instructions {
    margin-top: 10px;
    font-size: 14px;
    color: #666;
}

.submit-button {
    display: block;
    width: 100%;
    padding: 10px;
    background-color: #9c1e0e;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.submit-button:hover {
    background-color: #69160b;
}
</style>
