<template>
  <div class="todo">
    <div class="colorline"/>
    <div class="todobg"/>
    <div v-if="!editing">
      <div class="todotext"> {{ content }} </div>
      <div class="controlButtons">
        <button class="controlButtonEdit material-icons" @click="nowEditing">edit</button>
        <button class="controlButtonDone material-icons" :class="{'controlButtonDoneCompleted': !progress}" @click="toggleTodo">done</button>
        <button class="controlButtonCancel material-icons" @click="rem">cancel</button>
      </div>
    </div>
    <div v-else>
      <div class="todotext">
        <input class="todoinput" @keydown.enter="updateTodo" v-model="newContent"/>
      </div>
      <div class="controlButtons">
          <button class="controlButtonAdd material-icons" @click="updateTodo">done</button>
          <button class="controlButtonCancel material-icons" @click="cancelUpdateTodo">cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  props: ['content', 'id', 'progress'],
  emits: ['changedProgress', 'remmed', 'updatedContent'],
  setup(props, { emit }){

    const editing = ref(false)
    const newContent = ref('')

    function toggleTodo(){
      emit('changedProgress', props.id)
    }
    function rem(){
      emit('remmed', props.id)
    }
    function nowEditing(){
      editing.value = true
      newContent.value = props.content
    }
    function updateTodo(){
      if(newContent.value.length > 0){
        emit('updatedContent', props.id, newContent.value)
        newContent.value = ''
        editing.value = false
      }
    }
    function cancelUpdateTodo(){
      editing.value = false
    }

    return{ toggleTodo, rem, editing, newContent, nowEditing, updateTodo, cancelUpdateTodo }
  }
}
</script>

<style scoped>
.todo{
  position: relative;
  height: 100px;
  width: 700px;
  margin: auto;
  text-align: center;
}
.todobg{
  position: absolute;
  top: 0;
  left: 1%;
  z-index: -1;
  margin: auto ;
  background: whitesmoke;
  border-radius: 20px;
  height: 100%;
  width: 99%;
}
.colorline{
  position: absolute;
  top: 2%;
  left: 0;
  margin: auto;
  z-index: -2;
  margin: auto ;
  background: coral;
  border-radius: 20px;
  height: 96%;
  width: 100%;
}
.todotext{
  position: absolute;
  margin-left: 8%;
  display: flex;
  justify-content: left;
  text-align: left;
  align-items: center;
  height: 100%;
  width: 67%;
}
.controlButtons{
  margin-right: 5%;
  position: absolute;
  top: 0;
  right: 0;
  width: 20%;
  height: 100%;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.controlButtonDone,  .controlButtonCancel, .controlButtonEdit, .controlButtonAdd{
  color: black;
  cursor: pointer;
  padding: 0;
  margin: 0;
  border: none;
  text-align: center;
  background: whitesmoke;
  transition: all 0.2s ease;
}
.controlButtonDone:hover,  .controlButtonCancel:hover, .controlButtonEdit:hover, .controlButtonAdd:hover{
  transition: all 0.2s ease;
  transform: scale(1.2);
}
.controlButtonDoneCompleted, .controlButtonAdd{
  color: green;
}

.controlButtonCancel{
  color: red;
}
.todoinput{
    color: rgb(60, 60, 60);
    width: 100%;
    border-radius: 20px;
    border: 1px lightslategray solid;
    padding-top: 10px;
    padding-bottom: 10px;;
    padding-left: 10px;
    padding-right: 10px;
    transition: all 0.2s ease;
    background: whitesmoke;
}
.todoinput:focus{
    transition: all 0.2s ease;
    transform: scale(1.025);
    border: 2px lightslategray solid;
    outline: none;
}
</style>