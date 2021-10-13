<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <Header @receive="addTodo" />
        <List :todos="todos" />
        <Footer
          :todos="todos"
          @checkAllTodo="checkAllTodo"
          @clearDone="clearDone"
        />
      </div>
    </div>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import List from "./components/List.vue";
export default {
  name: "App",
  components: {
    Header,
    Footer,
    List,
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  mounted() {
    // 全局事件总线
    // this.$bus.$on('checkTodo',this.checkTodo)
    // this.$bus.$on('deleteTodo',this.deleteTodo)
    // 订阅
    this.pubId1 = pubsub.subscribe("checkTodo", (_, data) => {
      this.checkTodo(data);
    });
    this.pubId2 = pubsub.subscribe("deleteTodo", (_, data) => {
      this.deleteTodo(data);
    });
  },
  beforeDestroy() {
    // this.$bus.$off('checkTodo')
    // this.$bus.$off('deleteTodo')
    // pubsub.unsubscribe(this.pubId1);
    // pubsub.unsubscribe(this.pubId2);
    pubsub.unsubscribe(this.pubId2,this.pubId1);
  },
  watch: {
    todos: {
      deep: true,
      handler(newVal) {
        localStorage.setItem("todos", JSON.stringify(newVal));
      },
    },
  },
  methods: {
    // 添加一个todo
    addTodo(todoObj) {
      this.todos.unshift(todoObj);
    },
    // 勾选or取消勾选todo
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.done = !todo.done;
        }
      });
    },
    // 删除todo
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => {
        return todo.id !== id;
      });
    },
    // 全选or取消全选
    checkAllTodo(done) {
      this.todos.forEach((todo) => {
        todo.done = done;
      });
    },
    // 清除已完成
    clearDone() {
      this.todos = this.todos.filter((todo) => {
        return !todo.done;
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
/* base */
body {
  background: #fff;
}
.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}
.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(123, 198, 228);
  margin-right: 5px;
}
.btn-edit:hover {
  color: #fff;
  background-color: rgb(123, 198, 228);
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}
.btn:focus {
  outline: none;
}
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
