<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

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
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>  
    <ListaDeTarefas :tarefas="getTarefasFiltadras()"/> 
  </div>
</template>


