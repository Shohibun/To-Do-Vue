<script>
export default {
  methods: {
    onSubmit() {
      if (this.label === "") {
        return;
      }
      this.$emit("todo-added", this.label);
      this.label = "";
    },
  },
  data() {
    return {
      label: "",
    };
  },
};
</script>

<template>
  <form action="" @submit.prevent="onSubmit">
    <input
      type="text"
      name="new-todo"
      autocomplete="off"
      v-model.lazy.trim="label"
      placeholder="Add you activity"
      class="border-black border w-full px-2 mb-2 rounded"
      required
    />
    <button
      type="submit"
      class="w-full text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:focus:ring-gray-700 dark:border-gray-700"
    >
      Add
    </button>
  </form>
</template>
