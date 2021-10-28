<template>
  <div class="leaf" :class="{ selected }" @click="select">
    {{ leaf.type === 'link' ? '🔗' : '📝' }} {{ leaf.name }}
  </div>
</template>

<script>
export default {
  name: 'Leaf',
  props: {
    leaf: Object,
    path: Array
  },
  data: function() {
    return {
      selected: false
    }
  },
  methods: {
    select: function() {
      this.selected = true;
      this.updateSelected(this);
      this.updatePath([...this.path, this.leaf.name]);
    }
  },
  inject: ['updateSelected', 'updatePath']
}
</script>

