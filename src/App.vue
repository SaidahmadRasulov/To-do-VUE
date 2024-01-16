<template>
  <div>
    <HomeView @check="handleChange" :checked="checked" @filter="handleFilter" :todos="todos" @add="handleAdd" />
  </div>
</template>

<script>
import HomeView from "./views/HomeView.vue";

export default {
  data() {
    return {
      todos: [],
      removed: [],
      filter: 'new',
      checked: false
    };
  },
  mounted() {
    const storedTodos = localStorage.getItem("todos");
    this.todos = storedTodos ? JSON.parse(storedTodos) : [];
  },
  watch: {
    todos: {
      handler(newTodos) {
        localStorage.setItem("todos", JSON.stringify(newTodos));
      },
      deep: true,
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
      }
    },
    handleFilter(val) {
      this.filter = val;
      console.log(this.filter)
    },
    handleChange(id) {
      this.todos.map((item) => {
        if(item.id === id) {
        return this.checked = !this.checked
        }
      })
    }
  },
  computed: {
    filtered() {
      if(filter == 'new') {
        console.log('new')
      } else if(filter == 'rem') {
        console.log('removed')
      } else if(filter == 'don') {
        console.log('done')
      } else {
        console.log('all')
      }
    }
  },
  components: { HomeView },
};
</script>
