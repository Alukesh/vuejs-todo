<script setup>
  import {ref} from 'vue'
  import TodoList from './components/TodoList.vue'
  import DefaultBtn from './components/DefaultBtn.vue'

  let list = ref([
    {
      id:0,
      text: 'нужно сходить в магазин',
      isDone: true,
      isImportant: false,
      isChanging: false,
      isDeleted: false,
    },
    {
      id:2,
      text: 'нужно найти стул',
      isDone: false,
      isImportant: true,
      isChanging: false,
      isDeleted: false,
    },
    {
      id:3,
      text: 'не забыть удалить',
      isDone: false,
      isImportant: true,
      isChanging: false,
      isDeleted: true,
    },

  ]);
  const newLi = ref(null);
  const listStatus = ref('');

  const addToList = (arr) =>{
    if( !newLi.value.trim().length) return;

    arr.isDone = false;
    arr.isImportant = false;
    arr.isChanging = false;
    arr.isDeleted = false;

            list.value.push(arr);
    newLi.value =''
  };

  const removeFromList = (arr) =>{
    const idx = list.value.findIndex( i => i.id === arr.id);

    if (idx > -1) {
      console.log(JSON.stringify(list.value.splice(idx, 1)))
    }
  };
  const deletedFromList = (arr) =>{
    const idx = list.value.findIndex( i => i.id === arr.id);

    if (idx > -1) {
      list.value.splice(idx, 1, {...arr, isDeleted: !arr.isDeleted})
    }
  };

  const importantFromList = (arr) =>{
    const idx = list.value.findIndex(i => i.id === arr.id);

    if (idx > -1)
      list.value.splice(idx, 1, {...arr, isImportant: !arr.isImportant})
     // list.value = list.value.map(li => li.id === arr.id ? {...li, isImportant:!li.isImportant} : li);
  };

  const doneFromList = (arr) =>{
    const idx = list.value.findIndex(i => i.id === arr.id);

    if (idx > -1)
      list.value.splice(idx, 1 , {...arr, isDone: !arr.isDone})
  };

  const changeFromList = (arr) =>{
    const idx = list.value.findIndex(i => i.id === arr.id);

    if (idx > -1)
       list.value.splice(idx, 1, {...arr, isChanging: !arr.isChanging})

  };

  const saveChangeFromList = (textLi, item) =>{
    if (textLi.trim().length){
      const idx = list.value.findIndex(i => i.id === item.id);

    if (idx > -1)
      list.value.splice(idx, 1 ,{...item, text: textLi})

    // list.value = list.value.map(li => li.id === item.id ? {...li, text:textLi} : li);
    }
  }


  
</script>



<template>
  <header class="text-center ">
  </header>

  <main class="h-screen flex bg-green-300">
    <div class=" py-6 px-16 rounded-lg flex-col m-auto border-2 bg-lime-100">
      <h1 class="text-3xl text-center mb-4">todo</h1>


      <form @submit.prevent="addToList({id: Date.now(), text:newLi})" class="mb-6 flex gap-2 justify-center relative w-2/3 mx-auto">
        <input v-model="newLi" class="border-black text-center  block w-2/3 p-2 border-y-2 border-t-0" type="search" placeholder="add new task">
        <default-btn class="text-white absolute right-0 bg-black">Add</default-btn>

      </form>


      <div v-if="!listStatus" class="max-h-64 overflow-y-auto">
          <ul v-if="list.length" class="">

              <todo-list v-for="item in list.filter(li => !li.isDone && !li.isDeleted)" :key="item.id"
                         class="" :listArr="list" :item="item"
                         @remove="deletedFromList(item)"
                         @important=" importantFromList(item)"
                         @done=" doneFromList(item)"
                         @changing=" changeFromList(item)"
                         @changed="(textLi, item) => saveChangeFromList(textLi, item)"
              />
          </ul>

          <div v-else class="text-center text-lg">
            No todos here
          </div>

          <ul class="">
            <todo-list v-for="item in list.filter(li => li.isDone && !li.isDeleted)" :key="item.id"
                       class="" :listArr="list" :item="item"
                       @remove="deletedFromList(item)"
                       @important=" importantFromList(item)"
                       @done=" doneFromList(item)"
                       @changing=" changeFromList(item)"
                       @changed="(textLi, item) => saveChangeFromList(textLi, item)"
            />
          </ul>
      </div>

      <div v-else>
        <ul v-if="list.filter(li => listStatus === 'Done' ? li.isDone : listStatus === 'Important' ? li.isImportant : listStatus === 'Deleted' ? li.isDeleted : '').length" class="">
          <todo-list v-for="item in list.filter(li => listStatus === 'Done' ? li.isDone && !li.isDeleted: listStatus === 'Important' ? li.isImportant && !li.isDeleted: listStatus === 'Deleted' ? li.isDeleted : '' )"
                     :key="item.id"
                     :listArr="list" :item="item"
                     @remove="listStatus === 'Deleted' ? removeFromList(item) : deletedFromList(item)"
                     @important=" importantFromList(item)"
                     @done=" doneFromList(item)"
                     @changing=" changeFromList(item)"
                     @changed="(textLi, item) => saveChangeFromList(textLi, item)"
          />
        </ul>
        <div v-else>no tasks been {{listStatus}}</div>
        {{listStatus}}
      </div>


      <div class="flex gap-4 mt-6">
        <default-btn class="w-24 text-center bg-stone-400" :class="{'bg-green-400': listStatus === ''}" @click="listStatus = ''">All</default-btn>
        <default-btn class="w-24 text-center bg-stone-400" :class="{'bg-green-400': listStatus === 'Done'}" @click="listStatus = 'Done'">Done</default-btn>
        <default-btn class="w-24 text-center bg-stone-400" :class="{'bg-green-400': listStatus === 'Important'}" @click="listStatus = 'Important'">Important</default-btn>
        <default-btn class="w-24 text-center bg-stone-400" :class="{'bg-green-400': listStatus === 'Deleted'}" @click="listStatus = 'Deleted'">Deleted</default-btn>
      </div>

    </div>
  </main>


</template>





<style scoped>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.bg-green-400{
  transition: all .5s;
  background-color:  rgb(74 222 128 );
  color: white;
}
</style>
