<template>
  <div id="app">
    <Tree :tree="tree" v-if="Object.keys(tree).length > 0"/>
    <div v-else>loading tree ... </div>
  </div>
</template>

<script>
import Tree from './components/Tree.vue'

export default {
  name: 'App',
  components: {
    Tree
  },
  data: function() {
    return {
      tree: {}
    }
  },
  methods: {
    fetchTree: async function() {
      const rawTree = await fetch('/static/node_modules.json');
      this.tree = await rawTree.json();
    }
  },
  created: function() {
    this.fetchTree();
  }
}
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
</style>
