<template>
  <div id="app">
    <div class="path" v-if="treeHasItems">/ {{ path.join(' / ') }} </div>
    <Tree :tree="tree" :path="[]" v-if="treeHasItems"/>
    <div v-else>loading ... </div>
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
      tree: {},
      selectedItem: null,
      path: []
    }
  },
  computed: {
    treeHasItems: function() {
      return Object.keys(this.tree).length > 0;
    }
  },
  methods: {
    fetchTree: async function() {
      const rawTree = await fetch('/static/node_modules.json');
      const tree = await rawTree.json();
      this.tree = Object.freeze(tree);
      this.path.push(this.tree.name);
    },
    updateSelected: function(el) {
      if(this.selectedItem && this.selectedItem !== el) this.selectedItem.selected = false;
      this.selectedItem = el;
    },
    updatePath: function(path) {
      this.path = path;
    }
  },
  created: function() {
    this.fetchTree();
  },
  provide() {
    return {
      updateSelected: this.updateSelected,
      updatePath: this.updatePath
    }
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
  box-sizing: border-box;
  margin: 0 auto;
  padding: 40px;
  max-height: 800px;
  max-width: 600px;
  overflow: auto;
  background: #eff;
}
.path {
  margin-bottom: 10px;
  text-align: left;
  background: #eee;
}
</style>
