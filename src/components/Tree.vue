<template>
  <div class="tree">
    <div class="directory" :class="{ selected }" @click="onDirClick">📁 {{ tree.name }}</div>
    <div class="directory-content" :class="{ collapsed }" v-if="!collapsed">
      <Tree
        v-for="directory of directories"
        :key="directory.name"
        :tree="directory"
        :path="[...path, tree.name]">
      </Tree>
      <Leaf v-for="leaf of leaves" :key="leaf.name" :leaf="leaf" :path="[...path, tree.name]"></Leaf>
    </div>
  </div>
</template>

<script>
import Leaf from './Leaf.vue';

export default {
  name: 'Tree',
  components: {
    Leaf
  },
  props: {
    tree: Object,
    path: Array
  },
  data: function() {
    return {
      collapsed: true,
      selected: false
    }
  },
  computed: {
    leaves: function() {
      const leaves = this.tree.contents.filter((treeItem) => treeItem.type !== 'directory' ? treeItem : false);
      return Object.freeze(leaves);
    },
    directories: function() {
      const dirs = this.tree.contents.filter((treeItem) => treeItem.type === 'directory' ? treeItem : false);
      return Object.freeze(dirs);
    }
  },
  methods: {
    onDirClick: function() {
      this.collapsed = !this.collapsed;
      this.selected = true;
      this.updateSelected(this);
      this.updatePath([...this.path, this.tree.name]);
    }
  },
  inject: ['updateSelected', 'updatePath']
}
</script>

<style scoped>
.directory, .leaf {
  cursor: pointer;
  padding: 2px 6px;
  border-radius: 5px;
  white-space: nowrap;
}
.directory-content {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding-left: 10px;
}
.tree {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.selected {
  background: #b7f4e0;
}
</style>
