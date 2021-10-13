<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span
      >/全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "Footer",
  props: ["todos"],
  computed: {
    total() {
      return this.todos.length;
    },
    doneTotal() {
      /* const x = this.todos.reduce((pre,current)=>{
          return pre+(current.done?1:0)
        },0)
        return x */
      return this.todos.reduce((acc, todo) => acc + (todo.done ? 1 : 0), 0);
    },
    isAll: {
      get() {
        return this.total === this.doneTotal && this.total > 0;
      },
      set(val) {
        // this.checkAllTodo(val);
        this.$emit('checkAllTodo',val)
      },
    },
  },
  methods: {
    checkAll(e) {
      // this.checkAllTodo(e.target.checked);
      this.$emit('checkAllTodo',e.target.checked)
    },
    clearAll() {
      if (confirm("清除已完成？")) {
        // this.clearDone();
        this.$emit('clearDone')
      }
    },
  },
};
</script>

<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}
.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}
.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}
.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
