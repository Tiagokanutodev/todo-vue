<script setup>
import { reactive } from 'vue';

const estado = reactive  //O objeto estado é criado e contém uma propriedade tarefas, que é um array de objetos representando diferentes tarefas.
({  
  filtro: 'todas', 
  tarefaTemp: '',
  tarefas:   //tarefas, que é um array de objetos representando diferentes tarefas. Cada tarefa possui uma propriedade
    [
      {
        titulo: 'Estudar ES6',    //titulo (uma string que descreve a tarefa) e uma propriedade
        finalizada: false,        //finalizada (um booleano indicando se a tarefa está finalizada ou não).
      },
      {
        titulo: 'Estudar SASS',   //titulo (uma string que descreve a tarefa) e uma propriedade
        finalizada: false,        //finalizada (um booleano indicando se a tarefa está finalizada ou não).
      },
      {
        titulo: 'Ir para a academia',   //titulo (uma string que descreve a tarefa) e uma propriedade
        finalizada: true,               //finalizada (um booleano indicando se a tarefa está finalizada ou não).
      },
    ]
})

  const getTarefasPendentes = () =>
  {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada) 
  }

  const getTarefasFinalizadas = () =>
  {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltadras = () =>
  {
    const { filtro } = estado;

    switch (filtro)
    {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':  
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }

  const cadastraTarefa = () =>
  {
    const tarefaNova = 
    {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-5">  <!--pading, margin-boton, margin top, cor, borda arredondada, -->
      <h1>Minhas tarefas</h1>
      <p>
        Voce possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>  
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descriçao da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>    
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltadras()">
        <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo"  type="checkbox">
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .done
  {
    text-decoration: line-through; /*aplica uma linha através do conteúdo de texto de um elemento.*/
  }
</style>
