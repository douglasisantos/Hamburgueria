<template>
  <!-- <Message :msg="msg" v-show="msg" /> -->
  <div>
    <form id="burger-form" method="POST" @submit.prevent="createBurger">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome" />
      </div>

      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o Seu pão:</option>
          <option v-for="p in paes" :key="p.id" :value="p.tipo">
            {{ p.tipo }}
          </option>
        </select>
      </div>

      <div class="input-container">
        <label for="carne">Escolha a carne do seu Burger:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o tipo de carne:</option>
          <option v-for="c in carnes" :key="c.id" :value="c.tipo">
            {{ c.tipo }}
          </option>
        </select>
      </div>

      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo" />
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>

      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar meu Burguer" />
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const nome = ref('')
const pao = ref('')
const carne = ref('')
const opcionais = ref([])
const status = ref('solicitado')

const paes = ref([])
const carnes = ref([])
const opcionaisdata = ref([])

const getIngredientes = async () => {
  const req = await fetch('http://localhost:3000/ingredientes')
  const data = await req.json()
  paes.value = data.paes
  carnes.value = data.carnes
  opcionaisdata.value = data.opcionais
}

onMounted(() => {
  getIngredientes()
})
</script>

<style scoped>
#burger-form {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: 0 auto;
}
.input-container {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
}
label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}
#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}
#opcionais-title {
  width: 100%;
}
.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}
.checkbox-container span,
.checkbox-container input {
  width: auto;
}
.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}
.submit-btn {
  background-color: #222;
  color: #fcba03;
  border: 2px solid #222;
  padding: 10px;
  cursor: pointer;
  font-weight: bold;
  font-size: 16px;
  margin: 0 auto;
  transition: 0.5s;
}
.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
