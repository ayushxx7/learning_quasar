<template>
  <q-page class="bg-grey-3 column">
    <q-list class="bg-white"
      separator
      bordered>
      <q-item 
        v-for='(task, index) in tasks'
        :key="task.title"
        @click='task.done = !task.done'
        :class="{ 'done bg-blue-1': task.done }"
        clickable
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
        <q-item-section
          v-if="task.done" side>
        <q-btn 
          @click.stop="deleteTask(index)"
          flat round dense
          color="primary" 
          icon="delete"
          />
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
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Would you like to Delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify({
          message: 'Task Deleted',
          color: 'red'
        })
      })
    }
  }
}
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
