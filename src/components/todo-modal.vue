<template>
  <div @click.self="close()" v-show="isModalOpen" class="fixed-overlay">
    <div class="modal">
      <div class="modal__container">
        <h3 class="modal__title">Создать новую задачу</h3>
        <div>
          <button @click="close()" class="modal__close-btn btn">
            <svg width="22" height="22" viewBox="0 0 22 22" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="22" height="22" rx="5" fill="#314B99" /><path d="M8 8L11 11M14 14L11 11M11 11L14 8M11 11L8 14" stroke="white" stroke-linecap="round" /></svg>
          </button>
        </div>
        <label class="modal__label" for="">Описание</label>
        <input
          @keyup.enter="if (newTaskName) add();"
          v-model="newTaskName"
          ref="newTaskName"
          type="text"
          class="modal__input"
          placeholder="Введите описание"/>
        <button
          @click="add"
          :disabled="!newTaskName"
          class="modal__add-todo btn">
          Создать
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isModalOpen: {
      type: Boolean,
      require: true,
    },
  },
  data() {
    return {
      newTaskName: "",
    };
  },

  mounted() {
    document.addEventListener("keydown", this.escapeHandler);
  },
  beforeUnmount() {
    document.addEventListener("keydown", this.escapeHandler);
  },

  methods: {
    escapeHandler(evt) {
      if (this.isModalOpen && evt.key === "Escape") {
        this.close();
      }
    },
    close() {
      this.$emit("close");
    },
    add() {
      this.$emit("addTask", this.newTaskName);
      this.newTaskName = "";
      this.close();
    },
  },

  watch: {
    isModalOpen() {
      if (this.isModalOpen) {
        let self = this;
        this.$nextTick().then(() => {
          self.$refs.newTaskName.focus();
        });
      }
    },
  },
};
</script>

<style>
</style>