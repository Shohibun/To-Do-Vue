<script>
import ToDoItemEditForm from "./ToDoItemEditForm.vue";
export default {
  components: {
    ToDoItemEditForm,
  },
  props: {
    title: {
      required: true,
      type: String,
    },
    description: {
      required: true,
      type: String,
    },
    place: {
      required: true,
      type: String,
    },
    date: {
      required: true,
      type: String,
    },
    time: {
      required: true,
      type: String,
    },
    done: {
      default: false,
      type: Boolean,
    },
    id: {
      required: true,
      type: String,
    },
  },
  data() {
    return {
      isEditing: false,
    };
  },
  computed: {
    isDone() {
      return this.done;
    },
  },
  methods: {
    deleteToDo() {
      this.$emit("item-deleted");
    },
    toggleToItemEditForm() {
      // console.log(this.$refs.editButton);
      this.isEditing = true;
    },
    itemEditedTitle(newTitle) {
      this.$emit("item-edited-title", newTitle);
      this.isEditing = false;
      this.focusOnEditButton();
    },
    itemEditedDescription(newDescription) {
      this.$emit("item-edited-description", newDescription);
      this.isEditing = false;
      this.focusOnEditButton();
    },
    itemEditedPlace(newPlace) {
      this.$emit("item-edited-place", newPlace);
      this.isEditing = false;
      this.focusOnEditButton();
    },
    itemEditedDate(newDate) {
      this.$emit("item-edited-date", newDate);
      this.isEditing = false;
      this.focusOnEditButton(); 
    },
    itemEditedTime(newTime) {
      this.$emit("item-edited-time", newTime);
      this.isEditing = false;
      this.focusOnEditButton();
    },
    editCancelled() {
      this.isEditing = false;
      this.focusOnEditButton();
    },
    focusOnEditButton() {
      this.$nextTick(() => {
        const editButtonRef = this.$refs.editButton;
        editButtonRef.focus();
      });
    },
  },
};
</script>

<template>
  <div class="">
    <div class="pb-5" v-if="!isEditing">
      <div class="grid grid-cols-12 mb-4">
        <div class="col-span-1">
          <div class="flex justify-center items-center h-full">
            <div class="">
              <input
                type="checkbox"
                :id="id"
                :checked="isDone"
                @change="$emit('checkbox-changed')"
                value=""
                class="w-5 h-5 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
              />
            </div>
          </div>
        </div>

        <div class="col-span-11">
          <div class="mb-1">
            <label :for="id" class="ms-2 text-base font-bold dark:text-gray-950 text-slate-50">{{
              title
            }}</label>
          </div>

          <div class="mb-1">
            <label :for="id" class="ms-2 text-sm font-medium dark:text-gray-950 text-slate-50">{{
              description
            }}</label>
          </div>

          <div class="mb-1">
            <label :for="id" class="ms-2 text-sm font-medium dark:text-gray-950 text-slate-50">{{
              place
            }}</label>
          </div>

          <div class="mb-1">
            <label :for="id" class="ms-2 text-sm font-medium dark:text-gray-950 text-slate-50">{{
              date
            }}</label>
          </div>

          <div class="mb-1">
            <label :for="id" class="ms-2 text-sm font-medium dark:text-gray-950 text-slate-50">{{
              time
            }}</label>
          </div>
        </div>
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div class="col-span-1">
          <button
            type="button"
            ref="editButton"
            @click="toggleToItemEditForm"
            class="w-full text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
          >
            Edit
            <!-- <span class="visually-hidden">{{ label }}</span> -->
          </button>
        </div>

        <div class="col-span-1">
          <button
            type="button"
            @click="deleteToDo"
            class="w-full focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900"
          >
            Delete
            <!-- <span class="visually-hidden">{{ label }}</span> -->
          </button>
        </div>
      </div>
    </div>
    <to-do-item-edit-form
      v-else
      :id="id"
      :title="title"
      :description="description"
      :place="place"
      :date="date"
      :time="time"
      @item-edited-title="itemEditedTitle"
      @item-edited-description="itemEditedDescription"
      @item-edited-place="itemEditedPlace"
      @item-edited-date="itemEditedDate"
      @item-edited-time="itemEditedTime"
      @edit-cancelled="editCancelled"
    ></to-do-item-edit-form>
  </div>
</template>
