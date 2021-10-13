<template>
  <li @click="handleCheck(todoObj.id)">
    <label>
      <input
        type="checkbox"
        :checked="todoObj.done"
        @change="handleCheck(todoObj.id)"
      />
      <!-- vue不建议直接修改props，直接修改对象会报错，修改对象的属性不报错 -->
      <!-- <input type="checkbox" v-model="todoObj.done" /> -->
      <span v-show="!todoObj.isEdit">{{ todoObj.title }}</span>
      <input v-show="todoObj.isEdit" type="text" ref="inputTitle" v-model="todoObj.title" @blur="handleBlur(todoObj,$event)">
    </label>
    <button class="btn btn-danger" @click="handleDelete(todoObj.id)">
      删除
    </button>
     <button v-show="!todoObj.isEdit" class="btn btn-edit" @click="handleEdit(todoObj)">
      编辑
    </button>
  </li>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  name: "Item",
  props: ["todoObj"],
  methods: {
    handleCheck(id) {
      // this.checkTodo(id);
      // 全局事件总线
      // this.$bus.$emit('checkTodo',id)
      // 发布
      pubsub.publish("checkTodo", id);
    },
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        // this.deleteTodo(id);
        // 全局事件总线
        // this.$bus.$emit('deleteTodo',id)
        // 发布
        pubsub.publish("deleteTodo", id);
      }
    },
    handleEdit(todo){
      todo.isEdit = true
      this.$nextTick(function(){
        this.$refs.inputTitle.focus()
      })
    },
    handleBlur(todo){
      todo.isEdit = false
      // todo.title = e.target.value
    }
  },
};
</script>

<style lang="less" scoped>
/*item */
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}
li label {
  float: left;
  cursor: pointer;
}
li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}
li button {
  float: right;
  display: none;
  margin-top: 3px;
}
li:before {
  content: initial;
}
li:last-child {
  border-bottom: none;
}
li:hover {
  background-color: #ddd;
}
li:hover button {
  display: block;
}
</style>
