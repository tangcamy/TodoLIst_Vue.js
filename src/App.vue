<script setup>
import AddTodo from "./components/AddTodo.vue"
import { v4 as uuidv4 } from 'uuid'
import { ref,reactive } from 'vue'
let NewTodoList = ref('')
const Lists = reactive([])

function addTodoList(){
    // console.log(NewTodoList.value)
    if (NewTodoList.value !== "") {
      const item = {
        id: uuidv4(),
        title: NewTodoList.value,
        completed: false,
      };
      Lists.unshift(item);
      NewTodoList.value = "";
  }
}

// 收到AddTodo.vue emit廣播的id進行處理
function removeItem(id) {
  // 找到原本資料集id的位置
  const item = Lists.findIndex((list) => {
    return list.id === id
  })
  // 刪除idindex，splice(index,刪除幾個)
  Lists.splice(item, 1)
}

const toggleDoneHandler = (id) => {
  const findTodo = Lists.find((todo) => todo.id === id);
  if (findTodo) {
    findTodo.completed = !findTodo.completed;
  }

}


</script>

<template>
  <main class="container mx-auto">
        <header class="m-2">
          <h1 class="text-6xl font-thin select-none">TODO!</h1>
          <div class="font-semibold select-none text-neutral-600">simple and studid todo app</div>
        </header>
        <form class="px-10 py-12 bg-white shadow-sm">
          <section class="flex">
            <input type="text" placeholder="做點重要的事吧..." class="w-full text-2xl focus:outline-none input-lg input input-bordered" v-model="NewTodoList" />
            <button @click.prevent="addTodoList" class="text-xl btn-lg btn btn-neutral" >新增</button>
          </section>
        </form>

        <section class="px-10 py-6 mt-4 bg-white">
          <ul class="">
            <AddTodo @remove-item="removeItem" @toggleDone="toggleDoneHandler" v-for="l in Lists"  :list="l" />
          </ul>
        </section>
      </main>
</template>

<style scoped>
button:hover{
  opacity: 0.8;
}

</style>
