<template>
  <li>
    <label>
      <input type="checkbox" :checked="todoObj.done" @change="checkedChange(todoObj.id)" />
      <!-- <input v-show="!todoObj.isEdit" type="checkbox" /> -->
      <input
        type="text"
        ref="inputText"
        v-show="todoObj.isEdit"
        :value="todoObj.title"
        @blur="handleBlur(todoObj, $event)"
      />
      <span v-show="!todoObj.isEdit">{{ todoObj.title }}</span>
    </label>
    <button class="btn btn-danger" @click="todoDelete(todoObj.id)">删除</button>
    <button class="btn btn-edit" v-show="!todoObj.isEdit" @click="handleEdit(todoObj)">编辑</button>
  </li>
</template>

<script>
import pubsub from 'pubsub-js'
export default {
  name: "MyItem",
  props: ["todoObj"],
  methods: {
    checkedChange(id) {
      this.$bus.$emit("todoChange", id)

    },
    todoDelete(id) {
      if (confirm("是否删除"))
        // this.$bus.$emit("todoDel", id)
        pubsub.publish("todoDel", id)
    },
    handleEdit(todoObj) {
      // console.log("handleEdit被调用了")
      if (todoObj.hasOwnProperty("isEdit")) {
        console.log("有isEdit属性")
        todoObj.isEdit = false

        if (todoObj.isEdit === false) {
          todoObj.isEdit = true
        } else {
          todoObj.isEdit = false
        }
      } else {
        console.log("没有isEdit属性")

        this.$set(todoObj, "isEdit", true)
      }
      this.$nextTick(function () {
        this.$refs.inputText.focus()
      })
      // setTimeout(() => {
      //   this.$refs.inputText.focus()
      // }, 200)

    },
    handleBlur(todoObj, e) {
      // this.$set(todoObj, "isEdit", false)
      if (
        e.target.value.trim() == "") return alert("输入不能为空")
      todoObj.isEdit = false
      this.$bus.$emit("updataTodo", todoObj.id, e.target.value)

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
</style>