<template>
  <div class="todo-footer" v-show="todoLen">
    <label>
      <!-- <input type="checkbox" :checked="checkedAll" @change="isAll" /> -->
      <input type="checkbox" v-model="checkedAll" />
    </label>
    <span>
      <span>已完成{{ countDone }}</span>
      / 全部{{ todoLen }}
    </span>
    <button class="btn btn-danger" @click="clearChecked">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos"],
  methods: {
    // isAll(e) {
    //   // console.log(e.target.checked)
    //   this.checkedIsAll(e.target.checked);
    // },
    clearChecked() {
      // this.todos.done =!this.todos.done
      this.todos.forEach(todo => {
        // console.log(todo.done)
        todo.done = false

      });
    }
  },
  computed: {
    countDone() {
      return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0);
    },
    todoLen() {
      return this.todos.length;
    },
    checkedAll: {
      get() {
        return this.countDone === this.todoLen && this.todoLen > 0;

      },
      set(value) {
        this.$emit("checkedIsAll", value)
      }
    }

  },



};
</script>

<style>
/*footer*/
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