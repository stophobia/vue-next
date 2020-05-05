<template lang="pug">
div.container
  h1.text-center.mb-3 List
  div.row
    div(
        v-bind:class="selected ? 'col-8' : 'col-12'"
    )
      div
        p.alert.alert-danger(
            v-if="checkIfAlreadyExists"
          ) '{{ text }}' is already exists.
        div.border.d-flex
          input.w-100.border-0.py-2.pl-3(
            type="text"
            placeholder="+ Add a Task"
            v-model="text"
            v-on:keypress.enter="addToDo"
          )
        div.border.d-flex.align-items-center(
          v-for="toDo in filteredList"
          v-bind:key="toDo.createdAt"
        )
          input.mx-3(
            type="checkbox"
            v-model="toDo.done"
          )
          div.w-100.border-0.p-2(
            style="cursor: pointer;"
            v-bind:class="toDo.done ? 'task-completed' : ''"
            v-on:click="openDetails(toDo)"
          ) {{ toDo.text }}
          button.btn.btn-danger.btn-sm.mr-1(
            v-on:click="deleteToDoAndResetSelected(toDo.createdAt)"
          ) Delete
    div.border(
      style="height: 240px;"
      v-if="selected"
      v-bind:class="selected ? 'col-4' : 'd-none'"
    )
      div.d-flex.align-items-center.py-2
        span.flex-grow-1.font-weight-bold Todo Details
        button.btn(
          v-on:click="closeDetails"
        ) X
      div.d-flex.align-items-center
        input.mx-3(
          type="checkbox"
          v-model="selected.done"
        )
        input.border-0(
          type="text"
          v-bind:class="selected.done ? 'task-completed' : ''"
          v-model="selected.text"
        )
      hr
      div.d-flex.flex-column.align-items-end.mb-3
        small Created At: {{ new Date(selected.createdAt).toDateString() }} {{ new Date(selected.createdAt).toLocaleTimeString() }}
      div.d-flex
        div.flex-grow-1
        button.btn.btn-danger(
          v-on:click="deleteToDoAndResetSelected(selected.createdAt)"
        ) Delete
</template>

<script>
import { useDetailCard } from '@/compositions/detail-card';
import { useList } from '@/compositions/list';

export default {
  setup() {
    const { text, search, filteredList, checkIfAlreadyExists, addToDo, toDoList, deleteToDo} = useList();
    const { selected, openDetails, closeDetails, resetSelected } = useDetailCard();
    
    const deleteToDoAndResetSelected = (createdAt) => {
      deleteToDo(createdAt);
      resetSelected(createdAt);
    }
    return {
      text,
      search,
      toDoList,
      selected,
      filteredList,
      checkIfAlreadyExists,
      addToDo,
      openDetails,
      closeDetails,
      deleteToDoAndResetSelected
    }
  }
}
</script>

<style lang="sass" scoped>
.task-completed
  text-decoration: line-through
  color: gray
</style>