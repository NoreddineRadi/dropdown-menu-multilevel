
<template>
  <div id="app">
    <div>
      <div class="d-flex justify-content-center">
      <div class="w-25">
      <TreeNode
        role="" 
        :list="listItems"
        :offsetX="14" 
        :offsetY="-4"
        hideIcon
        className="my-class" 
        classNameChild="my-class-child"
        @clickOnItem="clickOnItem($event)"
      >
        <span slot="btn" class="material-icons">more_vert</span>
      </TreeNode>
    </div>
    </div>
    </div>
  </div>
</template>

<script>
import TreeNode from './TreeNode.vue';
export default {
  name: 'App',
  props: {
  },
  components: { 
    TreeNode,
  },
  data() {
    return {
      listItems: [{
        label: 'action long', icon: 'palette', align: 'right', children: [{ label: 'action 1 - level 1', icon: 'home', align: 'right', children: [] }],
      },
      {
        label: 'action 2',
        icon: 'home',
        align: 'left',
        children: [
          { label: 'action 1 - level 1 noradi', align: 'right', offsetX: 14,  children: [{ label: 'action 1 - level 2', align: 'right', children: [] }] },
          {
            label: 'action 2 - level 1',
            align: 'right',
            children: [{
              label: 'action 1 - level 2',
              align: 'right',
              children: [{ label: 'action 1 - level 3', align: 'right', children: [] }],
            }],
          },
          { label: 'action 3 - level 1', align: 'right', children: [] }],
      },
      {
        label: 'action 3', icon: 'wb_auto', align: 'right', children: [{ label: 'action 3 - level 1', align: 'right', children: [] }],
      },
      { label: 'action 4', align: 'right', icon: 'delete', children: [] }],
    }
  }, 
  methods: {
    clickOnItem(el) {
     // eslint-disable-next-line no-console
      console.log('item app', el)
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.my-class {
  padding: 5px
}
.my-class-bp__btn {
  border: none;
  background: white;
}

.my-class-bp__btn--active {
  background: #fff;
}
.my-class-bp__body {
  width: 12rem; 
  padding: 0.6rem; 
  font-size: 1rem;
  min-height: 5rem;
}
</style>
