<script setup>
import { reactive } from 'vue';

  const estado = reactive ({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: []
  })

  const getPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getPendentes();
      case 'finalizadas':
        return getFinalizadas();
        default:
          return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getPendentes().length }} tarefas pendentes
      </p>
    </header>
  <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemp" @change="e => estado.tarefaTemp = e.target.value" required class="form-control" type="text" placeholder="Descrição da tarefa">
      </div>
      <div class="col-md-2">
        <button class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="e => estado.filtro = e.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getFiltradas()">
      <input @change="e => tarefa.finalizada = e.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>
</div>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
