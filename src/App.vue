<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo" />
        <MyList :todos="todoscpd" />
        <MyFooter :todos="todoscpd" @checkedIsAll="checkedIsAll" />
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
      let newTitle = this.todos.find(item => todoTxt.title === item.title)
      console.log(newTitle)

      if (!newTitle) {
        this.todos.unshift(todoTxt)
      } else {
        alert("请不要输入已经存在内容" + newTitle.title)
      }
      // !newTitle && this.todos.unshift(todoTxt);

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
        return todo.id !== id
      });
    },
    // 全选或者全不选
    checkedIsAll(done) {
      this.todos.forEach((todo) => {
        todo.done = done
      })
    },
    // 更新数据
    updataTodo(id, title) {
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.title = title
        }
      })
    }

  },
  computed: {
    todoscpd() {
      let arr = []
      this.todos.forEach((todo) => {
        let bol = arr.find(item => item.title === todo.title)
        !bol && arr.push(todo)
      })
      return arr
    }
  },
  mounted() {
    this.$bus.$on("todoChange", this.todoChange)
    this.pubId = pubsub.subscribe("todoDel", this.todoDel)
    this.$bus.$on("updataTodo", this.updataTodo)

  },
  beforeDestroy() {
    this.$bus.$off(["todoChange", "updataTodo"])
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
.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(60, 122, 146);
  margin-right: 5px;
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

  