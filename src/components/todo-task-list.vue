<template>
  <div class="todos-list">
    <div class="todos-list__header">
      <div class="todos-list__change-status"></div>
      <div class="todos-list__name">Описание</div>
      <div class="todos-list__status">Статус</div>
      <div class="todos-list__date">Дата</div>
    </div>
    <div
      v-for="(task, index) in filteredTasks"
      :key="index"
      class="todos-list__line">
      <div class="todos-list__change-status">
        <button @click="toggleStatus(task)" class="change-status-container btn">
          <svg v-if="task.status === 'Выполнено'" class="todos-list__change-status-pic" width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="10" cy="10" r="9.5" fill="white" stroke="#134EC1" /><path d="M6 9L10 14.5L14.5 5" stroke="#134EC1" stroke-linecap="round" stroke-linejoin="round" /></svg>
          <svg v-else class="btn" width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="10" cy="10" r="9.5" stroke="#16191D" /></svg>
        </button>
      </div>
      <div class="todos-list__name">{{ task.name }}</div>
      <div
        :class="'todos-list__status--' + (task.status === 'Выполнено' ? 'complete' : 'inwork')"
        class="todos-list__status">
        {{ task.status }}
      </div>
      <div class="todos-list__date">{{ task.date }}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    filteredTasks: {
      type: Array,
    },
  },
  methods: {
    toggleStatus(task) {
      this.$emit("toggleStatus", task);
    },
  },
};
</script>

<style>
</style>