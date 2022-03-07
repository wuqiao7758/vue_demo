<template>
  <transition name="todo" appear>
    <li>
      <label>
        <input type="checkbox" :checked="todo.done" @change="checkedChange(todo.id)" />
        <!-- <input v-show="!todo.isEdit" type="checkbox" /> -->
        <input
          type="text"
          ref="inputText"
          v-show="todo.isEdit"
          :value="todo.title"
          @blur="handleBlur(todo, $event)"
        />
        <span v-show="!todo.isEdit">{{ todo.title }}</span>
      </label>
      <button class="btn btn-danger" @click="todoDelete(todo.id)">删除</button>
      <button class="btn btn-edit" v-show="!todo.isEdit" @click="handleEdit(todo)">编辑</button>
    </li>
  </transition>
</template>

<script>
import pubsub from 'pubsub-js'
export default {
  name: "MyItem",
  props: ["todo"],
  methods: {
    checkedChange(id) {
      this.$bus.$emit("todoChange", id)

    },
    todoDelete(id) {
      if (confirm("是否删除"))
        // this.$bus.$emit("todoDel", id)
        pubsub.publish("todoDel", id)
    },
    handleEdit(todo) {
      // console.log("handleEdit被调用了")
      if (todo.hasOwnProperty("isEdit")) {
        console.log("有isEdit属性")
        todo.isEdit = false

        if (todo.isEdit === false) {
          todo.isEdit = true
        } else {
          todo.isEdit = false
        }
      } else {
        console.log("没有isEdit属性")

        this.$set(todo, "isEdit", true)
      }
      this.$nextTick(function () {
        this.$refs.inputText.focus()
      })
      // setTimeout(() => {
      //   this.$refs.inputText.focus()
      // }, 200)

    },
    handleBlur(todo, e) {
      // this.$set(todo, "isEdit", false)
      if (
        e.target.value.trim() == "") return alert("输入不能为空")
      todo.isEdit = false
      this.$bus.$emit("updataTodo", todo.id, e.target.value)

    }

  },
};
</script>

<style scoped>
/*item*/
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
li:hover {
  background-color: #ccc;
}

li:hover button {
  display: block;
}
li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

.todo-enter-active {
  animation: atguigu 0.5s linear;
}
.todo-leave-active {
  animation: atguigu 0.5s linear reverse;
}

@keyframes atguigu {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0px);
  }
}
</style>