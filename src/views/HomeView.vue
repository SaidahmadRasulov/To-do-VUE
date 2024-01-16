<template>
  <div class="container w-[1200px] mx-auto">
    <Control :checked="checked" @filter="handleFilter" @add="handleAdd" />
    <div class="box w-1/2 mx-auto p-4 shadow-lg rounded-lg bg-orange-300">
      <div class="list py-4 text-white" v-for="item in todos">
        <div class="checked flex items-center gap-4" v-if="checked">
          <div class="checkbox p-2 border-2 border-gray-600 rounded-md"
            @click="handleToggle(item.id)"
          ></div>
          <h1>{{ item.title }}</h1>
        </div>
        <div class="checked flex items-center gap-4" v-else>
          <div class="checkbox p-2 border-2 border-gray-600 rounded-md"
            @click="handleToggle(item.id)"
          ></div>
          <h1 class="line-through">{{ item.title }}</h1>
        </div>
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
  },
  methods: {
    handleAdd(val) {
      this.$emit("add", val);
    },
    handleFilter(val) {
      this.$emit("filter", val);
    },
    handleToggle(id) {
      this.$emit('check', id)
    },
  },
  components: { Control },
};
</script>
<style lang=""></style>
