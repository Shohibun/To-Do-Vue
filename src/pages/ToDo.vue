<script>
import ToDoItem from "./ToDoItem.vue";
import ToDoForm from "./ToDoForm.vue";
import { nanoid } from "nanoid";
import { useDark, useToggle } from "@vueuse/core";

const isDark = useDark();
const toggleDark = useToggle(isDark);

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
          date: "2024-07-16",
          time: "12:34",
          done: true,
          timeStamp: Date.now(),
        },
        {
          id: "todo-" + nanoid(),
          title: "Learn Create To Do List",
          description: "Try for give up :) ",
          place: "Jember",
          date: "2024-07-12",
          time: "09:14",
          done: false,
          timeStamp: Date.now(),
        },
        {
          id: "todo-" + nanoid(),
          title: "Have Fun",
          description: "Have fun if this is done",
          place: "Bandung",
          date: "2024-07-18",
          time: "14:34",
          done: true,
          timeStamp: Date.now(),
        },
        {
          id: "todo-" + nanoid(),
          title: "I Hope This Works",
          description: "I hope it's done :(",
          place: "Bogor",
          date: "2024-07-22",
          time: "21:34",
          done: false,
          timeStamp: Date.now(),
        },
      ],
      filterOption: "all",
      sortOption: "addTime",
    };
  },
  methods: {
    addToDo({ title, description, place, date, time }) {
      this.ToDoItems.push({
        id: "todo-" + nanoid(),
        title: title,
        description: description,
        place: place,
        date: date,
        time: time,
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
    editToDoDate(toDoId, newDate) {
      const toDoEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoEdit.date = newDate;
    },
    editToDoTime(toDoId, newTIme) {
      const toDoEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoEdit.time = newTIme;
    },
    sortTasks() {
      if (this.sortOption === "alphabet") {
        this.ToDoItems.sort((a, b) => a.title.localeCompare(b.title));
      } else if (this.sortOption === "addTime") {
        this.ToDoItems.sort((a, b) => a.timeStamp - b.timeStamp);
      } else if (this.sortOption == "dueDate") {
        this.ToDoItems.sort((a, b) => new Date(a.date) - new Date(b.date));
      }
    },
    filterTasks() {
      // Hanya buat pemicu reaktivitasnya saja
    },
    toggleDark() {
      toggleDark();
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
      } else if (this.sortOption === "dueDate") {
        return filtered.sort((a, b) => new Date(a.date) - new Date(b.date));
      } else {
        return filtered;
      }
    },
  },
};
</script>

<template>
  <div class="w-full flex justify-center py-12">
    <div
      class="w-9/12 px-8 rounded border shadow-xl dark:bg-slate-50 bg-blue-950"
    >
      <label class="inline-flex items-center cursor-pointer mt-3">
        <input
          @change="toggleDark"
          type="checkbox"
          v-model="isDark"
          class="sr-only peer"
        />
        <div
          class="relative w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"
        ></div>
      </label>

      <!-- Jadi fungsi md: disini untuk responsive yang berlaku jika ukuran layar lebih dari 768px -->
      <div class="md:grid md:grid-cols-3 gap-4">
        <div class="col-span-1 md:border-r-4">
          <div class="w-full md:w-10/12 mx-auto mt-12">
            <h1
              class="text-3xl text-center font-bold dark:text-blue-800 text-slate-50"
            >
              MustDo
            </h1>
            <p class="text-base text-center dark:text-black text-slate-50">
              What needs to be done ?
            </p>

            <to-do-form @todo-added="addToDo"></to-do-form>
          </div>
        </div>

        <div class="col-span-2">
          <p
            class="text-base text-center font-bold mt-4 mb-2 dark:text-blue-800 text-slate-50"
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
                  class="block mb-2 text-base font-medium dark:text-gray-950 text-slate-50"
                  >Filter By:</label
                >
                <select
                  v-model="filterOption"
                  @change="filterTasks"
                  class="border border-blue-950 text-sm rounded-lg block w-full p-2.5 text-slate-50 dark:bg-blue-950 bg-sky-600"
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
                  class="block mb-2 text-base font-medium dark:text-gray-950 text-slate-50"
                  >Sort by:</label
                >
                <select
                  v-model="sortOption"
                  @change="sortTasks"
                  class="border border-blue-950 text-sm rounded-lg block w-full p-2.5 text-slate-50 dark:bg-blue-950 bg-sky-600"
                >
                  <option value="addTime">Time Added</option>
                  <option value="alphabet">Alphabetical</option>
                  <option value="dueDate">Due Date</option>
                </select>
              </div>
            </div>
          </div>

          <ul aria-labelledby="list-summary">
            <li v-for="item in sortedAndFilteredToDoItems" :key="item.id">
              <to-do-item
                :time="item.time"
                :date="item.date"
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
                @item-edited-date="editToDoDate(item.id, $event)"
                @item-edited-time="editToDoTime(item.id, $event)"
              >
              </to-do-item>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
