<template>
  <div class="tree">
    <div class="directory" @click="toggle">📁 {{ tree.name }}</div>
    <div class="directory-content" :class="{ collapsed }">
      <tree v-for="directory of directories" :key="directory.name" :tree="directory"></tree>
      <div class="leaf" v-for="leaf of leaves" :key="leaf.name">
        {{ leaf.type === 'link' ? '🔗' : '📝' }} {{ leaf.name }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tree',
  props: {
    tree: Object
  },
  data: function() {
    return {
      collapsed: true
    }
  },
  computed: {
    leaves: function() {
      return this.tree.contents.filter((treeItem) => treeItem.type !== 'directory' ? treeItem : false)
    },
    directories: function() {
      return this.tree.contents.filter((treeItem) => treeItem.type === 'directory' ? treeItem : false)
    }
  },
  methods: {
    toggle: function() {
      this.collapsed = !this.collapsed;
    }
  }
}
</script>

<style scoped>
.directory, .leaf {
  cursor: pointer;
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
.collapsed {
  display: none;
}
.selected {
  background: #b7f4e0;
}
</style>
