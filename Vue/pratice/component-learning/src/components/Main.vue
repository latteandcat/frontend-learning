<template>
  <div class="main">
    <h2>Main</h2>
  </div>
  <TabBar @click="clickTab"
          :titles="tabTitles"
          :active-tab="currentTab"></TabBar>
  <List :active-list="currentList"></List>
</template>

<script>
  import TabBar from './main/TabBar.vue'
  import List from './main/List.vue'
  export default {
    name: 'Main',
    components: {
      TabBar,
      List
    },
    data() {
      return {
        tabTitles: ['SectionA', 'SectionB', 'SectionC'],
        defaultLists: {
          'SectionA': ['a', 'a', 'a', 'a'],
          'SectionB': ['b', 'b', 'b', 'b'],
          'SectionC': ['c', 'c', 'c', 'c']
        },
        currentTab: 'SectionA',
        currentList: ['a', 'a', 'a', 'a']
      }
    },
    methods: {
      clickTab(item) {
        if (item === this.currentTab) return false
        console.log('click the', item)
        this.currentTab = item
        this.currentList = this.defaultLists[item]
      }
    }
  }
</script>

<style scoped>
  .main {
    background-color: #bbbaba;
    border-radius: 5px;
  }
</style>