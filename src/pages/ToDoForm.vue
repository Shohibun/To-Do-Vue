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
      <label
        for="title"
        class="block mb-2 text-sm font-medium dark:text-gray-950 text-slate-50"
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
      <label
        for="description"
        class="block mb-2 text-sm font-medium dark:text-gray-950 text-slate-50"
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
      <label
        for="place"
        class="block mb-2 text-sm font-medium dark:text-gray-950 text-slate-50"
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
      class="w-full text-slate-50 dark:bg-blue-950 bg-sky-600 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2"
    >
      Add
    </button>
  </form>
</template>
