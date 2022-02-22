<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask()"
        filled
        square
        placeholder="Add Task"
        class="col"
        bg-color="white"
        dense
      >
        <template v-slot:append>
          <q-btn color="black" @click="addTask()" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(item, index) in todoList"
        @click="item.done = !item.done"
        clickable
        :class="{ 'done bg-blue-1': item.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="item.done"
            color="primary"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ item.title }}</q-item-label>
        </q-item-section>
        <q-item-section side v-if="item.done">
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            dense
            round
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!todoList.length" class="absolute-center no-tasks bg-transparent">
      <q-icon name="check" size="100px" color="primary"
       />
       <div class="text-h5 text-primary q-mt-md text-center">
         No Task
       </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      todoList: [
        // {
        //   title: "Task 1",
        //   done: false,
        // },
        // {
        //   title: "Task 2",
        //   done: true,
        // },
        // {
        //   title: "Task 3",
        //   done: false,
        // },
      ],
    };
  },
  methods: {
    addTask() {
      this.todoList.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Would you delete?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.todoList.splice(index, 1);
          this.$q.notify({
            message: "Task Deleted",
            color: "purple",
          });
        });
    },
  },
};
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks{
  opacity: 0.5;

}
</style>
