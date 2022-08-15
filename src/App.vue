<script setup>
  import {ref, reactive} from 'vue'
  import TodoList from './components/TodoList.vue'

  let list = ref([
    {
      id:0,
      text: 'нужно сходить в магазин',
      isDone: false,
      isImportant: false,
      isChanging: false,
    },
    {
      id:2,
      text: 'нужно найти стул',
      isDone: false,
      isImportant: true,
      isChanging: false,
    },
  ]);
  const newLi = ref(null);

  const addToList = (arr) =>{
    if( !newLi.value.trim().length) return;

    arr.isDone = false;
    arr.isImportant = false;
    arr.isChanging = false;
            list.value.push(arr);
    newLi.value =''
  };

  const removeFromList = (arr) =>{
     list.value = list.value.filter(li => li.id !== arr.id);
     // alert(JSON.stringify(arr))
  };
  const importantFromList = (arr, status) =>{
     list.value = list.value.map(li => li.id === arr.id ? {...li, isImportant:!li.isImportant} : li);
  };
  const doneFromList = (arr) =>{
     list.value = list.value.map(li => li.id === arr.id ? {...li, isDone:!li.isDone} : li);
  };
  const changeFromList = (arr) =>{
    list.value = list.value.map(li => li.id === arr.id ? {...li, isChanging:!li.isChanging} : li);
    // alert(JSON.stringify(arr))
  };
  const saveChangeFromList = (textLi, item) =>{
    if (textLi.trim().length)
    list.value = list.value.map(li => li.id === item.id ? {...li, text:textLi} : li);
  }



  
</script>



<template>
  <header class="text-center ">
  </header>

  <main class="h-screen flex bg-green-300">
    <div class=" py-6 px-16 rounded-lg flex-col m-auto border-2 bg-lime-100">
      <h1 class="text-3xl text-center mb-4">todo</h1>


      <form @submit.prevent="addToList({id: Date.now(), text:newLi})" class="mb-6 flex gap-2 justify-center ">
        <input v-model="newLi" class="border-black text-center  block w-2/3 p-2 border-y-2 border-t-0" type="search" placeholder="add new task">
        <button class="bg-black rounded-lg p-2 text-white">ADD</button>
      </form>


      <ul v-if="list.length" class=" overflow-y-auto">

          <todo-list v-for="item in list.filter(li => !li.isDone)" :key="item.id"
                     class="" :listArr="list" :item="item"
                     @remove="removeFromList(item)"
                     @important=" importantFromList(item)"
                     @done=" doneFromList(item)"
                     @changing=" changeFromList(item)"
                     @changed="(textLi, item) => saveChangeFromList(textLi, item)"
          />


      </ul>
      <div v-else >
        no todo
      </div>

      <ul class=" overflow-y-auto" v-for="item in list.filter(li => li.isDone)" :key="item.id">
        <Transition >
          <todo-list class="" :listArr="list" :item="item"
                     @remove="removeFromList(item)"
                     @important=" importantFromList(item)"
                     @done=" doneFromList(item)"
                     @changing=" changeFromList(item)"
                     @changed=" saveChangeFromList(textLi, item)"
          />
        </Transition>

      </ul>
    </div>
  </main>


</template>





<style scoped>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
