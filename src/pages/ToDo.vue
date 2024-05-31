<script>
import ToDoItem from "./ToDoItem.vue";
import ToDoForm from "./ToDoForm.vue";
import { nanoid } from "nanoid";

export default {
  components: {
    ToDoItem,
    ToDoForm,
  },
  data() {
    return {
      ToDoItems: [
        {
          id: "todo-" + nanoid(),
          title: "Learn Vue",
          description: "Vue is a JavaScript framework",
          place: "Jakarta",
          done: true,
          timeStamp: Date.now(),
        },
        {
          id: "todo-" + nanoid(),
          title: "Learn Create To Do List",
          description: "Try for give up :) ",
          place: "Jember",
          done: false,
          timeStamp: Date.now(),
        },
        {
          id: "todo-" + nanoid(),
          title: "Have Fun",
          description: "Have fun if this is done",
          place: "Bandung",
          done: true,
          timeStamp: Date.now(),
        },
        {
          id: "todo-" + nanoid(),
          title: "I Hope This Works",
          description: "I hope it's done :(",
          place: "Bogor",
          done: false,
          timeStamp: Date.now(),
        },
      ],
      filterOption: "all",
      sortOption: "addTime",
    };
  },
  methods: {
    addToDo({ title, description, place }) {
      this.ToDoItems.push({
        id: "todo-" + nanoid(),
        title: title,
        description: description,
        place: place,
        done: false,
        timeStamp: Date.now(),
      });
      this.sortTasks();
      this.filterTasks();
    },
    deleteToDo(toDoId) {
      const itemIndex = this.ToDoItems.findIndex((item) => item.id === toDoId);
      this.ToDoItems.splice(itemIndex, 1);
      this.sortTasks();
      this.filterTasks();
      this.$refs.listSummary.focus();
    },
    updateDoneStatus(toDoId) {
      const toDoUpdate = this.ToDoItems.find((item) => item.id === toDoId);
      toDoUpdate.done = !toDoUpdate.done;
      this.filterTasks();
    },
    editTodoTitle(toDoId, newTitle) {
      const toDoEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoEdit.title = newTitle;
    },
    editTodoDescription(toDoId, newDescription) {
      const toDoEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoEdit.description = newDescription;
    },
    editToDoPlace(toDoId, newPlace) {
      const toDoEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoEdit.place = newPlace;
    },
    sortTasks() {
      if (this.sortOption === "alphabet") {
        this.ToDoItems.sort((a, b) => a.title.localeCompare(b.title));
      } else if (this.sortOption === "addTime") {
        this.ToDoItems.sort((a, b) => a.timeStamp - b.timeStamp);
      }
    },
    filterTasks() {
      // Hanya buat pemicu reaktivitasnya saja
    },
  },
  computed: {
    listSummary() {
      const numberFinishedItems = this.ToDoItems.filter(
        (item) => item.done
      ).length;
      return `${numberFinishedItems} out of ${this.ToDoItems.length} items completed`;
    },
    filteredToDoItems() {
      if (this.filterOption === "completed") {
        return this.ToDoItems.filter((item) => item.done);
      } else if (this.filterOption === "pending") {
        return this.ToDoItems.filter((item) => !item.done);
      } else {
        return this.ToDoItems;
      }
    },
    sortedAndFilteredToDoItems() {
      let filtered = this.filteredToDoItems;
      if (this.sortOption === "alphabet") {
        console.log("masuk sort abjad");
        return filtered.sort((a, b) => a.title.localeCompare(b.title));
      } else if (this.sortOption === "addTime") {
        console.log("masuk sort time");
        return filtered.sort((a, b) => a.timeStamp - b.timeStamp);
      } else {
        return filtered;
      }
    },
  },
};
</script>

<template>
  <div class="w-full flex justify-center py-12">
    <div class="w-9/12 px-8 rounded border shadow-xl bg-white">
      <!-- Jadi fungsi md: disini untuk responsive yang berlaku jika ukuran layar lebih dari 768px -->
      <div class="md:grid md:grid-cols-3 gap-4">
        <div class="col-span-1 md:border-r-4">
          <div class="w-full md:w-10/12 mx-auto mt-12">
            <h1 class="text-3xl text-center font-bold">MustDo</h1>
            <p class="text-base text-center">What needs to be done ?</p>

            <to-do-form @todo-added="addToDo"></to-do-form>
          </div>
        </div>

        <div class="col-span-2">
          <p
            class="text-base text-center font-bold mt-4 mb-2"
            id="list-summary"
            ref="listSummary"
            tabindex="-1"
          >
            {{ listSummary }}
          </p>

          <div class="md:grid md:grid-cols-2 gap-4 mb-9">
            <div class="col-span-1 mb-3">
              <form class="w-8/12 mx-auto">
                <label
                  for="filter"
                  class="block mb-2 text-base font-medium text-black"
                  >Filter By:</label
                >
                <select
                  v-model="filterOption"
                  @change="filterTasks"
                  class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                >
                  <option value="all" selected>All</option>
                  <option value="pending">Pending</option>
                  <option value="completed">Complete</option>
                </select>
              </form>
            </div>

            <div class="col-span-1">
              <div class="w-8/12 mx-auto">
                <label
                  for="sort"
                  class="block mb-2 text-base font-medium text-black"
                  >Sort by:</label
                >
                <select
                  v-model="sortOption"
                  @change="sortTasks"
                  class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                >
                  <option value="addTime">Time Added</option>
                  <option value="alphabet">Alphabetical</option>
                </select>
              </div>
            </div>
          </div>

          <ul aria-labelledby="list-summary">
            <li v-for="item in sortedAndFilteredToDoItems" :key="item.id">
              <to-do-item
                :place="item.place"
                :description="item.description"
                :title="item.title"
                :done="item.done"
                :id="item.id"
                @checkbox-changed="updateDoneStatus(item.id)"
                @item-deleted="deleteToDo(item.id)"
                @item-edited-title="editTodoTitle(item.id, $event)"
                @item-edited-description="editTodoDescription(item.id, $event)"
                @item-edited-place="editToDoPlace(item.id, $event)"
              >
              </to-do-item>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
