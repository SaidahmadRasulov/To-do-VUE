<script setup>
  import { ref } from 'vue';
</script>

<template>
  <div>
    <HomeView
      @check="handleChange"
      :checked="checked"
      @filter="handleFilter"
      :todos="filtered"
      @add="handleAdd"
      @delete="handleDelete"
      :isActive="isActive"
      @edit="handleEdit"
    />
  </div>
</template>

<script>
import HomeView from "./views/HomeView.vue";

export default {
  data() {
    return {
      todos: [],
      doneTodos: [],
      removedTodos: [],
      newTodos: [],
      filter: "all",
      checked: false,
      isActive: "all",
      isEdit: {},
      startEdit: false,
      inputRef: ref
    };
  },
  mounted() {
    const storedTodos = localStorage.getItem("todos");
    this.todos = storedTodos ? JSON.parse(storedTodos) : [];
    const newArray = localStorage.getItem("new-todos");
    this.newTodos = newArray ? JSON.parse(newArray) : [];
    const removedTodos = localStorage.getItem("rem-todos");
    this.removedTodos = removedTodos ? JSON.parse(removedTodos) : [];
    const doneArray = localStorage.getItem("don-todos");
    this.doneTodos = doneArray ? JSON.parse(doneArray) : [];
  },
  watch: {
    todos: {
      handler(newTodos) {
        localStorage.setItem("todos", JSON.stringify(newTodos));
      },
      deep: true,
    },
    newTodos: {
      handler(newArray) {
        localStorage.setItem("new-todos", JSON.stringify(newArray));
      },
      deep: true,
    },
    removedTodos: {
      handler(newTodos) {
        localStorage.setItem("rem-todos", JSON.stringify(newTodos));
      },
      deep: true,
    },
    doneTodos: {
      handler(newArray) {
        localStorage.setItem("don-todos", JSON.stringify(newArray));
      },
      deep: true,
    },
    handleAdd: {
      handler(newValue) {
        console.log(newValue);
      },
      deep: true,
    },
  },
  methods: {
    handleAdd(val) {
      if (!this.startEdit) {
        if (val.trim() !== "") {
          const newTodo = {
            id: Date.now(),
            title: val,
            checked: false,
          };
          this.todos.push(newTodo);
          this.newTodos.push(newTodo);
        }
        setTimeout(() => {
          this.newTodos = [];
        }, 40000);
      } else {
        val = this.isEdit.title;
        console.log(val);
      }
    },
    handleFilter(val) {
      this.filter = val;
      this.isActive = this.filter;
    },
    handleChange(id) {
      this.todos.map((item) => {
        if (item.id === id) {
          return (item.checked = !item.checked);
        }
        if (item.checked) {
          this.doneTodos.push(item);
          localStorage.setItem("don-todos", this.doneTodos);
        }
      });
      localStorage.setItem("todos", this.todos);
      localStorage.setItem("new-todos", this.newTodos);
      localStorage.setItem("rem-todos", this.removedTodos);
    },
    handleDelete(id) {
      this.todos = this.todos.filter((item) => {
        if (item.id === id) {
          this.removedTodos.push(item);
        } else {
          return this.todos;
        }
      });
      localStorage.setItem("todos", this.todos);
      localStorage.setItem("new-todos", this.newTodos);
      localStorage.setItem("rem-todos", this.removedTodos);
    },
    handleEdit(obj) {
      this.isEdit = obj;
      this.startEdit = true;
      this.handleAdd();
    },
  },
  computed: {
    filtered() {
      if (this.filter == "new") {
        return this.newTodos;
      } else if (this.filter == "rem") {
        return this.removedTodos;
      } else if (this.filter == "don") {
        return this.doneTodos;
      } else {
        return this.todos;
      }
    },
  },
  components: { HomeView },
};
</script>
