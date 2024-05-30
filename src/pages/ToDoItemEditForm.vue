<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      newLabel: this.label,
    };
  },
  methods: {
    onsubmit() {
      if (this.newLabel && this.newLabel !== this.label) {
        this.$emit("item-edited", this.newLabel);
      }
      console.log("Label baru:", this.newLabel);
    },
    onCancel() {
      this.$emit("edit-cancelled");
    },
  },
  mounted() {
    const labelInputRef = this.$refs.labelInput;
    labelInputRef.focus();
  },
};
</script>

<template>
  <form @submit.prevent="onsubmit">
    <div class="grid grid-rows-2 mb-3">
      <div class="row-span-1 mx-auto mb-1">
        <label class="mb-2 text-sm font-medium text-black">
          Edit Name for &quot;{{ label }}&quot;</label
        >
      </div>

      <div class="row-span-1 mx-auto">
        <input
          type="text"
          ref="labelInput"
          autocomplete="off"
          :id="id"
          v-model.lazy.trim="newLabel"
          class="px-3 border border-black rounded"
        />
      </div>
    </div>

    <div class="grid grid-cols-2 gap-4">
      <div class="col-span-1">
        <button
          type="button"
          @click="onCancel"
          class="w-full focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900"
        >
          Cancel
          <!-- <span class="visually-hidden">Editing {{ label }}</span> -->
        </button>
      </div>

      <div class="col-span-1">
        <button
          type="submit"
          class="w-full focus:outline-none text-white bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800"
        >
          Save
          <!-- <span class="visually-hidden">Edit for {{ label }}</span> -->
        </button>
      </div>
    </div>
  </form>
</template>
