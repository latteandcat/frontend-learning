<template>
  <div class="list">
    <h2>{{ this.title }}</h2>
    <ul>
      <li v-for="item in activeList"><span>{{ item }}</span></li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: 'List',
    props: ['activeList'],
    inject: ['title']
  }
</script>

<style scoped>
.list {
  min-height: 300px;
  margin: 10px 0;
  background-color: rgb(0, 221, 255);
  border-radius: 5px;
}
.list h2 {
  margin: 0;
  padding: 10px;
  text-align: left;
}
.list ul {
  margin: 0;
  padding: 10px 0;
}

.list li {
  margin: 0;
  padding: 0 10px;
  list-style: none;
  text-align: left;
  font-size: 18px;
}
</style>