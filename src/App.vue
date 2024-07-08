<template>
  <div class="h-3/6 bg-gray-100 mt-52 mr-20 ml-20">
    <div class="flex bg-white">
      <!-- Sol Taraf -->
      <div class="w-5/12 h-full p-4 bg-white shadow-md mr-10">
        <textarea
          class="w-full h-3/4 p-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
          placeholder="Yapacaklarınızı buraya yazınız..."
          rows="5"
        ></textarea>
        <button
          @click="addTodo"
          class="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
        >
          Ekle
        </button>
      </div>
      <!-- Sağ Taraf -->
      <div class="w-1/2">
        <h1 class="justify-center text-3xl mb-2 text-center text-secondary">
          Yapılacaklar Listesi
        </h1>
        <ul class="list-disc justify-between item-center">
          <li
            class="mr-4 flex ml-auto"
            v-for="(item, index) in list"
            :key="index"
            :class="{ 'line-through': item.done }"
          >
            {{ item.text }}
            <div class="flex ml-auto">
              <button class="text-right mr-2" @click="done(index)">
                <i class="pi pi-check"></i>
              </button>
              <button class="text-right" @click="deleteTodo(index)">
                <i class="pi pi-times"></i>
              </button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
.line-through {
  text-decoration: line-through;
}
</style>

<script setup>
import { ref, onMounted } from "vue";

const list = ref([]);

const addTodo = () => {
  const newTodo = document.querySelector("textarea");
  if (newTodo.value === "") {
    alert("Boş bırakılamaz");
  } else {
    list.value.push({ text: newTodo.value, done: false });
  }
  newTodo.value = "";
  newTodo.focus();
  // console.log(list.value);
  localStorage.setItem("my-items", JSON.stringify(list.value));
};

const done = (index) => {
  list.value[index].done = !list.value[index].done;
  // console.log(list.value);
  localStorage.setItem("my-items", JSON.stringify(list.value));
  console.log(localStorage.getItem("my-items"));
};

const deleteTodo = (index) => {
  list.value.splice(index, 1);
  // console.log(list.value);
  localStorage.setItem("my-items", JSON.stringify(list.value));
  console.log(localStorage.getItem("my-items"));
};

onMounted(() => {
  const items = localStorage.getItem("my-items");
  if (items) {
    list.value = JSON.parse(items);
  }
});
</script>
