<script>
export default {
  methods: {
    onSubmit() {
      if (this.title === "" || this.description === "" || this.place === "") {
        return;
      }

      this.$emit("todo-added", {
        title: this.title,
        description: this.description,
        place: this.place,
      });

      this.title = "";
      this.description = "";
      this.place = "";
    },
  },
  data() {
    return {
      title: "",
      description: "",
      place: "",
    };
  },
};
</script>

<template>
  <form action="" @submit.prevent="onSubmit" class="mt-5 mb-10">
    <div class="mb-2">
      <label for="title" class="block mb-2 text-sm font-medium text-black"
        >Title</label
      >
      <input
        type="text"
        name="new-todo"
        autocomplete="off"
        v-model.lazy.trim="title"
        class="border-black border w-full px-2 mb-2 rounded"
        required
      />
    </div>

    <div class="mb-3">
      <label for="description" class="block mb-2 text-sm font-medium text-black"
        >Description</label
      >
      <input
        type="text"
        name="new-todo"
        autocomplete="off"
        v-model.lazy.trim="description"
        class="border-black border w-full px-2 mb-2 rounded"
        required
      />
    </div>

    <div class="mb-3">
      <label for="place" class="block mb-2 text-sm font-medium text-black"
        >Place</label
      >
      <input
        type="text"
        name="new-todo"
        autocomplete="off"
        v-model.lazy.trim="place"
        class="border-black border w-full px-2 mb-2 rounded"
        required
      />
    </div>

    <button
      type="submit"
      class="w-full text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:focus:ring-gray-700 dark:border-gray-700"
    >
      Add
    </button>
  </form>
</template>
