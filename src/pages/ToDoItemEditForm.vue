<script>
export default {
  props: { // Data yang nanti diedit
    title: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    place: {
      type: String,
      required: true,
    },
    date: {
      type: String,
      required: true,
    },
    time: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },
  data() {
    return { // Mengembalikan data baru yang telah diedit
      newTitle: this.title,
      newDescription: this.description,
      newPlace: this.place,
      newDate: this.date,
      newTime: this.time,
    };
  },
  methods: {
    onsubmit() { // Fungsi yang akan dipanggil ketika tombol submit ditekan && mengecek apakah nilai data lama === data baru
      if (this.newTitle && this.newTitle !== this.title) {
        this.$emit("item-edited-title", this.newTitle);
      }
      if (this.newDescription && this.newDescription !== this.description) {
        this.$emit("item-edited-description", this.newDescription);
      }
      if (this.newPlace && this.newPlace !== this.place) {
        this.$emit("item-edited-place", this.newPlace);
      }
      if (this.newDate && this.newDate !== this.date) {
        this.$emit("item-edited-date", this.newDate);
      }
      if (this.newTime && this.newTime !== this.time) {
        this.$emit("item-edited-time", this.newTime);
      }
    },
    onCancel() {
      this.$emit("edit-cancelled");
    },
  },
  mounted() {
    // Mengakses referensi input
    const titleInputRef = this.$refs.titleInput;
    const descriptionInputRef = this.$refs.descriptionInput;
    const placeInputRef = this.$refs.placeInput;
    const dateInputRef = this.$refs.dateInput;
    const timeInputRef = this.$refs.timeInput;

    // Untuk memeriksa apakah data yang diinputkan tidak null atau undefined
    if (titleInputRef) {
      titleInputRef.focus()
    } else if (descriptionInputRef) {
      descriptionInputRef.focus()
    } else if (placeInputRef) {
      placeInputRef.focus()
    } else if (dateInputRef) {
      dateInputRef.focus()
    } else if (timeInputRef) {
      timeInputRef.focus()
    }
  },
};
</script>

<template>
  <form @submit.prevent="onsubmit">
    <div class="grid grid-cols-2 gap-2 mb-3">
      <!-- Title -->
      <div class="col-span-1">
        <label class="mb-2 text-sm font-medium dark:text-gray-950 text-slate-50">
          Edit for title:</label
        >
      </div>

      <div class="col-span-1">
        <input
          type="text"
          ref="titleInput"
          autocomplete="off"
          :id="id"
          v-model.lazy.trim="newTitle"
          class="px-3 border border-black rounded"
        />
      </div>
      <!-- Title -->

      <!-- Description -->
      <div class="col-span-1">
        <label class="mb-2 text-sm font-medium dark:text-gray-950 text-slate-50">
          Edit for description:</label
        >
      </div>

      <div class="col-span-1">
        <input
          type="text"
          ref="discriptionInput"
          autocomplete="off"
          :id="id"
          v-model.lazy.trim="newDescription"
          class="px-3 border border-black rounded"
        />
      </div>
      <!-- Description -->

      <!-- Place -->
      <div class="col-span-1">
        <label class="mb-2 text-sm font-medium dark:text-gray-950 text-slate-50">
          Edit for place:</label
        >
      </div>

      <div class="col-span-1">
        <input
          type="text"
          ref="placeInput"
          autocomplete="off"
          :id="id"
          v-model.lazy.trim="newPlace"
          class="px-3 border border-black rounded"
        />
      </div>
      <!-- Place -->

      <!-- Date -->
      <div class="col-span-1">
        <label class="mb-2 text-sm font-medium dark:text-gray-950 text-slate-50">
          Edit for date:</label
        >
      </div>

      <div class="col-span-1">
        <input
          type="date"
          ref="dateInput"
          autocomplete="off"
          :id="id"
          v-model.lazy.trim="newDate"
          class="px-3 border border-black rounded"
        />
      </div>
      <!-- Date -->

      <!-- Time -->
      <div class="col-span-1">
        <label class="mb-2 text-sm font-medium dark:text-gray-950 text-slate-50">
          Edit for time:</label
        >
      </div>

      <div class="col-span-1">
        <input
          type="time"
          ref="timeInput"
          autocomplete="off"
          :id="id"
          v-model.lazy.trim="newTime"
          class="px-3 border border-black rounded"
        />
      </div>
      <!-- Time -->
    </div>

    <div class="grid grid-cols-2 gap-4">
      <div class="col-span-1">
        <button
          type="button"
          @click="onCancel"
          class="w-full focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900"
        >
          Cancel
        </button>
      </div>

      <div class="col-span-1">
        <button
          type="submit"
          class="w-full text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
        >
          Save
        </button>
      </div>
    </div>
  </form>
</template>
