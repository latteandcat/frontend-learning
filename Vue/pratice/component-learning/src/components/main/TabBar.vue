<template>
  <div class="tab">
    <div v-for="(item, index) in titles"
        :key="index"
        class="item"
        :class="{ active: item === activeTab }"
        @click="$emit('click', item)">
      <span>{{ item }}</span>
    </div>
  </div>
</template>

<script>
  export default {
    emits: ['click'],
    props: ['titles', 'activeTab'],
  }
</script>

<style scoped>
.tab {
  display: flex;
  justify-content: space-between;
}

.tab .item {
  width: 150px;
  background-color: rgb(0, 179, 255);
  padding: 5px 5px;
  border-radius: 5px;
  cursor: pointer;
}

.tab .active {
  background-color: rgb(255, 132, 179);
}
</style>