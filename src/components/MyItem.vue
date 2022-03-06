<template>
  <li>
    <label>
      <input type="checkbox" :checked="todoObj.done" @change="checkedChange(todoObj.id)" />
      <span>{{ todoObj.title }}</span>
    </label>
    <button class="btn btn-danger" @click="todoDelete(todoObj.id)">删除</button>
  </li>
</template>

<script>
export default {
  name: "MyItem",
  props: ["todoObj"],
  methods: {
    checkedChange(id) {
      this.$bus.$emit("todoChange", id)

    },
    todoDelete(id) {
      this.$bus.$emit("todoDel", id)
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