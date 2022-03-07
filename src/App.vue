<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo" />
        <MyList :todos="todos" />
        <MyFooter :todos="todos" @checkedIsAll="checkedIsAll" />
      </div>
    </div>
  </div>
</template>

<script>
import pubsub from 'pubsub-js'
import MyHeader from "./components/MyHeader"
import MyList from "./components/MyList"
import MyFooter from "./components/MyFooter"
export default {
  name: "App",
  components: {
    MyHeader,
    MyList,
    MyFooter,
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    // 添加
    addTodo(todoTxt) {
      this.todos.unshift(todoTxt);
    },
    // 勾选
    todoChange(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.done = !todo.done;
      });
    },
    // 删除
    todoDel(_, id) {
      this.todos = this.todos.filter((todo) => {
        return todo.id !== id;
      });
    },
    // 全选或者全不选
    checkedIsAll(done) {
      this.todos.forEach((todo) => {
        todo.done = done
      })
    },

  },
  mounted() {
    this.$bus.$on("todoChange", this.todoChange)
    // this.$bus.$on("todoDel", this.todoDel)
    this.pubId = pubsub.subscribe("todoDel", this.todoDel)
  },
  beforeDestroy() {
    this.$bus.$off(["todoChange"])
    pubsub.unsubscribe(this.pubId)

  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value))
      }
    }
  }
};
</script>
<style>
/*base*/
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

  