<template>
  <q-page class="bg-grey-3 column">
    <q-list class="bg-white"
      separator
      bordered>
      <q-item 
        v-for='task in tasks'
        :key="task.title"
        @click='task.done = !task.done'
        clickable
        :class="{ 'done': task.done }"
        v-ripple>
        <q-item-section avatar>
          <q-checkbox 
            class='no-pointer-events'
            v-model="task.done" 
            color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
      </q-item>

    </q-list>  
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      tasks: [
        {
          title: "Get Oranage",
          done: false
        },
        {
          title: "Make Juice",
          done: false
        },
        {
          title: "Drink Juice",
          done: false
        },
      ]
    }
  },
  name: 'PageIndex'
}
</script>

<style>
.done {
  .q-item__label {
    text-decoration: line-through;

  }
}
</style>
