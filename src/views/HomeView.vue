<template>
  <div class="container w-[1200px] mx-auto">
    <Control
      :isActive="isActive"
      :checked="checked"
      @filter="handleFilter"
      @add="handleAdd"
    />
    <div class="box w-1/2 mx-auto p-4 shadow-lg rounded-lg bg-orange-300">
      <div
        class="list py-4 text-white"
        v-if="todos.length > 0"
        v-for="item in todos"
        :key="item.id"
      >
        <div
          class="checked flex items-center justify-between"
          v-if="item.checked"
        >
          <div class="flex items-center gap-4 w-4/5">
            <i
              class="bx bx-check-square cursor-pointer text-[36px]"
              @click="handleToggle(item.id)"
            ></i>
            <div class="w-full relative">
              <input
                type="text"
                :value="item.title"
                class="bg-inherit outline-none text-2xl w-full"
              />
              <Transition name="entering">
                <div class="line_div active"></div>
              </Transition>
            </div>
          </div>
          <div class="flex items-center gap-3 w-1/5">
            <i
              @click="handleEdit(item)"
              class="bx bx-pencil text-gray-500 text-lg hover:text-white cursor-pointer transition-colors delay-75"
            ></i>
            <i
              class="bx bx-trash text-red-600 text-lg hover:text-white cursor-pointer transition-colors delay-75"
              @click="handleDelete(item.id)"
            ></i>
          </div>
        </div>
        <div class="checked flex items-center justify-between" v-else>
          <div class="flex items-center gap-4 w-4/5">
            <i
              class="bx bx-checkbox cursor-pointer text-[36px]"
              @click="handleToggle(item.id)"
            ></i>
            <div class="relative w-full">
              <input
                type="text"
                :value="item.title"
                :key="item.id"
                class="bg-inherit outline-none text-2xl w-full"
              />
              <Transition name="out">
                <div class="line_div"></div>
              </Transition>
            </div>
          </div>
          <div class="flex items-center gap-3 w-1/5">
            <i
              @click="handleEdit(item)"
              class="bx bx-pencil text-gray-500 text-lg hover:text-white cursor-pointer transition-colors delay-75"
            ></i>
            <i
              class="bx bx-trash text-red-600 text-lg hover:text-white cursor-pointer transition-colors delay-75"
              @click="handleDelete(item.id)"
            ></i>
          </div>
        </div>
      </div>
      <div v-else class="text-center py-4 text-white text-2xl">
        <h1>Not Data</h1>
      </div>
    </div>
  </div>
</template>
<script>
import Control from "../components/Control.vue";

export default {
  data() {
    return {
      id: 0,
    };
  },
  props: {
    todos: {
      type: Array,
      required: true,
    },
    checked: {
      type: Boolean,
      required: true,
    },
    isActive: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleAdd(val) {
      this.$emit("add", val);
    },
    handleFilter(val) {
      this.$emit("filter", val);
    },
    handleToggle(id) {
      this.$emit("check", id);
    },
    handleDelete(id) {
      this.$emit("delete", id);
    },
    handleEdit(obj) {
      this.$emit("edit", obj);
    },
  },
  components: { Control },
};
</script>
<style>
.line_div::before {
  position: absolute;
  content: '';
  width: 0px;
  height: 2px;
  top: 50%;
  transition: all 3s ease-in-out;
}
.line_div.active::before{
  position: absolute;
  content: '';
  width: 100%;
  height: 2px;
  background-color: rgb(172, 13, 13);
  transition: all 3s ease-in-out;
}
</style>
