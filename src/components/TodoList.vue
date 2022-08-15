<script setup>
    import {ref, reactive} from 'vue'

const emit = defineEmits(['remove', 'important','done', 'changing', 'changed']);
const {listArr, item} = defineProps(['listArr', 'item']);
    const textLi = ref(null);

const saveChange = ()=>{
    emit('changing');
    emit('changed', textLi.value, item)
}

</script>


<template>
    <ul v-if="listArr.length" class="overflow-y-auto">
        <li class="flex items-center p-2 gap-4" >
            <label class="w-60 flex items-center">
                <input :checked="item.isDone" @change="emit('done', item)" value='skirt' class='list__checkbox' type="checkbox" />
                <span class="new_checkbox"/>
                <span v-if="!item.isChanging" class="mr-auto ml-4 w-52 inline-block leading-9"
                          :class="{'text-amber-500': item.isImportant,'text-stone-400':item.isDone, 'underline':item.isImportant }">
                    {{item.text}}
                </span>
                <input v-else v-model="textLi" class="bg-green-300 border-b-cyan-900" type="search">
            </label>

            <button v-if="item.isChanging" @click="saveChange()" class="bg-green-300 rounded-lg p-2 w-12">{{textLi ? 'Save' : 'Keep'}}</button>
            <button v-else @click="emit('changing')" class="bg-green-400 rounded-lg p-2 w-12">Edit</button>
            <button @click="emit('important', item)" class="bg-red-500 rounded-lg p-2">Important</button>
            <button @click="emit('remove',  item)">
                <img src="https://img.icons8.com/ios/50/000000/delete-forever--v1.png"/>
            </button>
        </li>
    </ul>
    <div v-else >
        no todo
    </div>

<!--    <ul class="todo__list" >-->
<!--        <li class="flex items-center p-2 gap-4" v-for="item in listArr.filter(li => li.isDone)">-->
<!--            <label class="w-60 flex items-center">-->
<!--                <input @change="emit('done', item)" value='skirt' class='list__checkbox' type="checkbox" />-->
<!--                <span class="new_checkbox"/>-->
<!--                <span class="mr-auto ml-4 w-52 inline-block leading-9"-->
<!--                      :class="{'text-amber-500':item.isImportant,'text-stone-400':item.isDone, 'underline':item.isImportant }">-->
<!--                    {{item.text}}-->
<!--                </span>-->
<!--            </label>-->

<!--            <button class="bg-green-300 rounded-lg p-2">Edit</button>-->
<!--            <button @click="emit('important', item)" class="bg-red-500 rounded-lg p-2">Important</button>-->
<!--            <button @click="emit('remove',  item)">-->
<!--                <img src="https://img.icons8.com/ios/50/000000/delete-forever&#45;&#45;v1.png"/>-->
<!--            </button>-->
<!--        </li>-->
<!--    </ul>-->



</template>



<style scoped>
    .list__checkbox{
        appearance: none;
        position: absolute;
    }
    .new_checkbox{
        width: 20px;
        height: 20px;
        border: 1px solid black;
        /*border-radius: 50%;*/
        position: relative;
        flex-shrink: 0;
        transition: all .3s;
    }
    .list__checkbox:checked + .new_checkbox::after{
        content: '';
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
        width: 12px;
        height: 12px;
        background-color: cadetblue;
    }
    .list__checkbox:focus + .new_checkbox::before{
        content: '';

        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;

        background-color: lightgrey;
    }



</style>