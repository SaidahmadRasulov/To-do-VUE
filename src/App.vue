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
      editVal: {},
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
    const activeClasses = localStorage.getItem("active-class");
    this.isActive = activeClasses ? JSON.parse(activeClasses) : "all";
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
    handleEdit: {
      handler(newObj) {
        const index = this.todos.findIndex((todo) => todo.id === newObj.id);
        if (index !== -1) {
          this.$set(this.todos, index, newObj);
        }
      },
      deep: true,
    },
    isActive: {
      handler(newClass) {
        localStorage.setItem("active-class", JSON.stringify(newClass));
      },
    },
  },
  methods: {
    handleAdd(val) {
      if (val.trim() !== "") {
        const newTodo = {
          id: Date.now(),
          title: val,
          checked: false,
        };
        this.todos.push(newTodo);
        this.newTodos.push(newTodo);
      }
      localStorage.setItem('new-todos', this.newTodos)
      setTimeout(() => {
        this.newTodos = [];
      }, 50000);
    },
    handleFilter(val) {
      this.filter = val;
      this.isActive = this.filter;
      localStorage.setItem("active-class", this.isActive);
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
      this.editVal = obj;
      localStorage.setItem("todos", this.todos);
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
