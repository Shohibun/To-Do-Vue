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
          label: "Learn Vue",
          done: true,
        },
        {
          id: "todo-" + nanoid(),
          label: "Learn Create To Do List",
          done: false,
        },
        {
          id: "todo-" + nanoid(),
          label: "Have Fun",
          done: true,
        },
        {
          id: "todo-" + nanoid(),
          label: "I Hope This Works",
          done: false,
        },
      ],
      filterOption: "all",
      sortOption: "addTime",
    };
  },
  methods: {
    addToDo(toDoLabel) {
      this.ToDoItems.push({
        id: "todo-" + nanoid(),
        label: toDoLabel,
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
    editToDo(toDoId, newLabel) {
      const toDoEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoEdit.label = newLabel;
    },
    sortTasks() {
      if (this.sortOption === "alphabet") {
        this.ToDoItems.sort((a, b) => a.label.localeCompare(b.label)); 
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
        console.log("masuk sort abjad")
        return filtered.sort((a, b) => a.label.localeCompare(b.label));
      } else if (this.sortOption === "addTime") {
        console.log("masuk sort time")
        return filtered.sort((a, b) => a.timeStamp - b.timeStamp);
      } else {
        return filtered;
      }
    },
  },
};
</script>

<template>
  <div class="w-full flex justify-center">
    <div class="w-4/12 px-8 rounded border shadow-xl">
      <h1 class="text-3xl text-center font-bold mb-4 mt-2">To-Do List</h1>
      <p class="text-base">What needs to be done ?</p>

      <to-do-form @todo-added="addToDo"></to-do-form>

      <p
        class="text-base text-center font-bold"
        id="list-summary"
        ref="listSummary"
        tabindex="-1"
      >
        {{ listSummary }}
      </p>

      <!-- Filter -->
      <form class="max-w-sm mx-auto mb-4">
        <label for="filter" class="block mb-2 text-base font-medium text-black"
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
      <!-- Filter -->

      <!-- Sort -->
      <div class="sort-container mb-12">
        <label for="sort" class="block mb-2 text-base font-medium text-black"
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
      <!-- Sort -->

      <ul aria-labelledby="list-summary">
        <li v-for="item in sortedAndFilteredToDoItems" :key="item.id">
          <to-do-item
            :label="item.label"
            :done="item.done"
            :id="item.id"
            @checkbox-changed="updateDoneStatus(item.id)"
            @item-deleted="deleteToDo(item.id)"
            @item-edited="editToDo(item.id, $event)"
          >
          </to-do-item>
        </li>
      </ul>
    </div>
  </div>
</template>
