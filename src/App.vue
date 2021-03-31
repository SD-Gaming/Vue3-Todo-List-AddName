<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <h1>Hello Vue 3.0 + Vite</h1>
  <h2 v-html="msg"></h2>
  <input type="text" v-model="itemNew" @keyup.enter="addNew" />
  <ul>
    <li
      v-for="(item, index) in items"
      :class="{ isStudent: item.student }"
      :key="index"
      @click="turnRed(item)"
      @contextmenu.prevent.stop="remove(index)"
    >{{ item.name }}</li>
  </ul>
</template>

<script lang="ts">
import { ref, reactive, watch, } from 'vue'

interface Student {
  name: string, student: boolean
}

export default {
  name: 'App',
  setup() {
    const itemNew = ref('')

    const storage = localStorage.getItem('student-list')
    let list = []
    if (storage) {
      list = JSON.parse(storage)
    }
    const items = reactive<Array<Student>>(list)
    const msg = 'Type name and mark who is student'

    function addNew() {
      if (itemNew.value === "") return
      items.push({
        name: itemNew.value,
        student: false
      })
      itemNew.value = ""
    }
    function turnRed(item: Student) {
      item.student = !item.student
    }
    function remove(index: number) {
      items.splice(index, 1)
    }

    watch(items, newval => {
      localStorage.setItem('student-list', JSON.stringify(newval))
    })
    return {
      msg, itemNew, items, addNew, turnRed, remove
    }
  }
}
</script>

<style>
.isStudent {
  color: red;
}
li:hover {
  cursor: pointer;
  user-select: none;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>