<template>
  <div class="w-1/2 mx-auto py-5">
    <div class="flex items-center">
        <input
      class="w-[90%] outline-none px-4 py-2 border-2 rounded-md"
      type="text"
      v-model="inputValue"
    />
    <button
      @click="handleAdd"
      class="rounded-md px-5 py-2 bg-green-400 text-white"
    >
      Add
    </button>
    </div>
    <div class="control_nav mt-4 flex items-center justify-center gap-10">
        <h2 @click="handleFilter('all')" class="py-2 px-10 text-white bg-orange-400">All</h2>
        <h2 @click="handleFilter('new')" class="py-2 px-10 text-white bg-orange-400">New</h2>
        <h2 @click="handleFilter('rem')" class="py-2 px-10 text-white bg-orange-400">Removed</h2>
        <h2 @click="handleFilter('don')" class="py-2 px-10 text-white bg-orange-400">Done</h2>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputValue: "",
    };
  },
  methods: {
    handleAdd() {
      if (this.inputValue !== "") {
        this.$emit("add", this.inputValue);
      }
      this.inputValue = "";
    },
    handleFilter(val) {
        this.$emit('filter', val)
    }
  },
};
</script>
<style lang=""></style>
