<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <form id="burger-form" @submit="createBurger">
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
        <label id="opcionais-title" for="opcionais"> Selecione os opcionais: </label>
        <div class="checkbox-container" v-for="op in opcionaisdata" :key="op.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="op.tipo" />
          <span>{{ op.tipo }}</span>
        </div>
      </div>

      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar meu Burguer" />
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import Message from './Message.vue'

// Dados reativos
const paes = ref<{ id: number; tipo: string }[] | null>(null)
const carnes = ref<{ id: number; tipo: string }[] | null>(null)
const opcionaisdata = ref<{ id: number; tipo: string }[] | null>(null)

const nome = ref<string>('')
const pao = ref<string>('')
const carne = ref<string>('')
const opcionais = ref<string[]>([])
const msg = ref<string>('')

// Buscar ingredientes
const getIngredientes = async () => {
  const req = await fetch('http://localhost:3000/ingredientes')
  const data = await req.json()
  paes.value = data.paes
  carnes.value = data.carnes
  opcionaisdata.value = data.opcionais
}

// Criar burger
const createBurger = async (e: Event) => {
  e.preventDefault()

  const data = {
    nome: nome.value,
    pao: pao.value,
    carne: carne.value,
    opcionais: Array.from(opcionais.value),
    status: 'Solicitado',
  }

  const dataJson = JSON.stringify(data)

  const req = await fetch('http://localhost:3000/burgers', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: dataJson,
  })

  const res = await req.json()

  msg.value = `Pedido N° ${res.id} criado com sucesso!`

  setTimeout(() => (msg.value = ''), 3000)

  // Resetar form
  nome.value = ''
  pao.value = ''
  carne.value = ''
  opcionais.value = []
}

// Chamar API ao montar
onMounted(() => {
  getIngredientes()
})
</script>

<style lang="scss" scoped>
$primary-color: #222;
$secondary-color: #fcba03;
$highlight-color: #cce5ff;
$highlight-border: #b8daff;

#burger-form {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: 0 auto;

  .input-container {
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;

    label {
      font-weight: bold;
      margin-bottom: 15px;
      color: $primary-color;
      padding: 5px 10px;
      border-left: 4px solid $secondary-color;
    }

    input,
    select {
      padding: 5px 10px;
      width: 300px;
    }
  }

  #opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;

    #opcionais-title {
      width: 100%;
    }

    .checkbox-container {
      display: flex;
      align-items: flex-start;
      width: 50%;
      margin-bottom: 20px;

      span,
      input {
        width: auto;
      }

      span {
        margin-left: 6px;
        font-weight: bold;
      }
    }
  }

  .submit-btn {
    background-color: $primary-color;
    color: $secondary-color;
    border: 2px solid $primary-color;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
    font-size: 16px;
    margin: 0 auto;
    transition: 0.5s;

    &:hover {
      background-color: transparent;
      color: $primary-color;
    }
  }
}
</style>
