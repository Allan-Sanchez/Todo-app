<template>
  <div class="q-pa-md q-gutter-sm">
    <q-editor
      v-model="editor"
      :definitions="{
        save: {
          tip: 'Save your work',
          icon: 'save',
          label: 'Save',
          handler: saveWork
        }
      }"
      :toolbar="[
        ['bold', 'italic', 'strike', 'underline'],
        ['upload', 'save']
      ]"
    />
    <q-card class="row" flat bordered v-for="(item, index) in tasks" :key="index">
      <q-card-section class="col q-mt-sm" :class="item.state ? 'underline' : ''" v-html="item.text" />
      <q-btn class="q-mt-sm q-ma-sm" round color="secondary" icon="check" @click="item.state = !item.state" />
      <q-btn class="q-mt-sm q-ma-sm" round color="negative" icon="delete" @click="deleteTask(index)" />
    </q-card>

    <div v-if="tasks.length == 0" class="flex flex-center">
      <p class="text-h4 q-mt-lg">No Tasks</p>
    </div>
  </div>
  
</template>

<script>
export default {
  data() {
    return {
      editor: '',
      tasks : [
        
      ]
    }
  },
  methods: {
    saveWork () {
      this.tasks.push({text:this.editor, state:false});
      this.editor = '';
      this.$q.notify({
        message: 'Task saved',
        color: 'green-4',
        textColor: 'white',
        icon: 'cloud_done'
      })
    },
    deleteTask (index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Do you really want to delete the task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        // console.log('>>>> OK')
        this.tasks.splice(index,1);
      })
    },
  }
}
</script>

<style>
  .underline{
    text-decoration: line-through;
    color: #ccc;
  }
</style>