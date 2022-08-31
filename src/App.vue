<template>
  <section>
    <div class="wrapper">
      <div class="header">
        <div class="header-line">
          <h2 class="header__title">To do list</h2>
          <button @click="isModalOpen = true" class="header__add-btn btn">
            <svg width="40" height="40" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="20" cy="20" r="20" fill="#D6DBEB" /><rect x="19" y="10" width="2" height="20" fill="#314B99" /><rect x="30" y="19" width="2" height="20" transform="rotate(90 30 19)" fill="#314B99" /></svg>
          </button>
        </div>
        <div class="header-line">
          <div class="header__search">
            <label for="search">
              <svg class="header__search-pic" width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M12.8645 11.3208H12.0515L11.7633 11.0429C12.7719 9.86964 13.3791 8.34648 13.3791 6.68954C13.3791 2.99485 10.3842 0 6.68954 0C2.99485 0 0 2.99485 0 6.68954C0 10.3842 2.99485 13.3791 6.68954 13.3791C8.34648 13.3791 9.86964 12.7719 11.0429 11.7633L11.3208 12.0515V12.8645L16.4666 18L18 16.4666L12.8645 11.3208ZM6.68954 11.3208C4.12693 11.3208 2.05832 9.25214 2.05832 6.68954C2.05832 4.12693 4.12693 2.05832 6.68954 2.05832C9.25214 2.05832 11.3208 4.12693 11.3208 6.68954C11.3208 9.25214 9.25214 11.3208 6.68954 11.3208Z" fill="#59BBA6"/></svg>
            </label>
            <input
              v-model="searchValue"
              id="search"
              class="header__input"
              type="text"
              placeholder="Поиск Имени, статуса или даты"
              autofocus/>
          </div>
          <div>
            <label for="sort">Сортировать по:</label>
            <select v-model="sortBy" class="header__select" name="" id="sort">
              <option disabled value="">Дата</option>
              <option value="date" selected>Дата</option>
              <option value="status">Статус</option>
              <option value="name">Имя</option>
            </select>
          </div>
        </div>
      </div>
      <todo-task-list
        :filtered-tasks="filteredTasks"
        @toggleStatus="toggleStatus"/>
      <todo-modal
        :is-modal-open="isModalOpen"
        @addTask="addNewTask"
        @close="isModalOpen = false"/>
    </div>
  </section>
</template>

<script>
import TodoModal from "@/components/todo-modal.vue";
import TodoTaskList from "@/components/todo-task-list.vue";

export default {
  name: "App",
  components: { TodoModal, TodoTaskList },
  
  data() {
    return {
      tasks: [],

      searchValue: "",
      sortBy: "",

      isModalOpen: false,
    };
  },
  created() {
    let tasksData = localStorage.getItem("my-todo-list");
    if (tasksData) {
      this.tasks = JSON.parse(tasksData);
    }
  },
  computed: {
    filteredTasks() {
      let tmpTasks = this.tasks;

      if (this.searchValue != "" && this.searchValue) {
        tmpTasks = tmpTasks.filter(
          (t) =>
            t.name.toLowerCase().includes(this.searchValue.toLowerCase()) ||
            t.date.includes(this.searchValue) ||
            t.status.toLowerCase().includes(this.searchValue.toLowerCase())
        );
      }

      tmpTasks = tmpTasks.sort((a, b) => {
        if (this.sortBy == "date") {
          return this.getDateToSort(a.date) - this.getDateToSort(b.date);
        } else if (this.sortBy == "status") {
          return a.status.localeCompare(b.status);
        } else if (this.sortBy == "name") {
          return a.name.localeCompare(b.name);
        }
      });
      return tmpTasks;
    },

    currentDate() {
      let d = new Date();
      let fullDate =
        (d.getDate() + "").padStart(2, "0") +
        "." +
        (d.getMonth() + 1 + "").padStart(2, "0") +
        "." +
        d.getFullYear();
      return fullDate;
    },
  },

  methods: {
    addNewTask(name) {
      let newTask = {
        name: name,
        status: "В работе",
        date: this.currentDate,
        id: this.tasks.length + 1,
      };

      this.tasks.push(newTask);
      this.updateLocalStorage("my-todo-list", this.tasks)
    },

    toggleStatus(task) {
      this.tasks.forEach((t) => {
        if (t.id === task.id) {
          t.status = t.status === "В работе" ? "Выполнено" : "В работе";
        }
        this.updateLocalStorage("my-todo-list", this.tasks)
      });
    },
    updateLocalStorage(name, data){
      localStorage.setItem(name, JSON.stringify(data));
    },
    getDateToSort(d) {
      return Date.parse(d.split(".").reverse().join("-"));
    },
  },
};
</script>

<style lang="scss" src="@/style.scss">
</style>