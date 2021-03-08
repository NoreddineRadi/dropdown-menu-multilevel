<template>
  <div id="app" class="d-flex">
    <div class="row w-100 wrapper">
      <div class="col col-sm-2 d-flex justify-content-start">
        <div class="">
          <TreeNode
            class="mx-3"
            :list="listItems"
            className="my-class"
            label="Dropdown"
            @clickOnItem="clickOnItem($event)"
            iconName="home"
          >
          </TreeNode>
        </div>
      </div>
      <div class="col col-sm-8 alert bg-secondary " @contextmenu="contextMenu">Right click to see the contextMenu !</div>
      <div class="col col-sm-2 d-flex justify-content-end">
        <div class="">
          <TreeNode
            class="mx-3"
            :list="listItems"
            hideChevron
            trigger="hover"
            @clickOnItem="clickOnItem($event)"
          >
          <span slot="btn-main" class="material-icons">more_vert</span>
          </TreeNode>
        </div>
      </div>
    </div>
    <div class="d-flex justify-content-start">
        <TreeNode
          ref="myContextMenu"
          asContextMenu
          trigger="hover"
          :list="listItems"
          className="my-class-menu"
          classNameChild="my-class-child"
          @clickOnItem="clickOnItem($event)"
         >
          <span slot="btn" class="material-icons">more_vert</span>
        </TreeNode>
      </div>
  </div>
</template>

<script>
import TreeNode from "./components/TreeNode.vue";
// eslint-disable-entire-file no-console
/* eslint-disable no-debugger, no-console */
export default {
  name: "App",
  props: {},
  components: {
    TreeNode,
  },
  data() {
    return {
      positions: {
        x: 0,
        y: 0,
      },
      listItems: [
        {
          label: "action long",
          icon: "palette",
          align: "",
          children: [
            {
              label: "action 1 - level 1",
              icon: "home",
              children: [],
            },
          ],
        },
        {
          label: "action 2",
          icon: "home",
          align: "right",
          children: [
            {
              label: "action 1 - level 1 noradi",
              align: "right",
              offsetX: 14,
              children: [
                { label: "action 1 - level 2", align: "right", children: [] },
              ],
            },
            {
              label: "action 2 - level 1",
              align: "right",
              children: [
                {
                  label: "action 1 - level 2",
                  align: "right",
                  children: [
                    {
                      label: "action 1 - level 3",
                      align: "right",
                      children: [],
                    },
                  ],
                },
              ],
            },
            { label: "action 3 - level 1", align: "right", children: [] },
             {
          label: "action long",
          icon: "palette",
          align: "bottom",
          children: [
            {
              label: "action 1 - level 1 hello",
              icon: "home",
              align: "right",
              children: [{
              label: "action 1 - level 1 hello",
              icon: "home",
              align: "right",
              children: [],
            }],
            },
          ],
        },
          ],
        },
        {
          label: "action 3",
          icon: "wb_auto",
          children: [
            { label: "action 3 - level 1", children: [] },
          ],
        },
        { label: "action 4", icon: "delete", className: 'single', children: [] },
      ],
    };
  },
  methods: {
    clickOnItem(el) {
      // eslint-disable-next-line no-console
      console.log("item app", el);
    },
    contextMenu(e) {
      this.$refs.myContextMenu.$children[0].isHidden = false
      this.$refs.myContextMenu.$children[0].$nextTick(()=> {
       this.$refs.myContextMenu.$children[0].top = e.clientY
      this.$refs.myContextMenu.$children[0].left = e.clientX
     }, this)
      e.preventDefault()
    }
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
.wrapper {
  height: 150px
}

.my-class {
  padding: 5px;
}
.my-class-bp__btn {
  background: lightgray;
  padding: 8px;
  border-radius: 8px;
  cursor: pointer

}
.single {
  padding-left: 2em;
}
.my-class-bp__body {
  width: 12rem;
  padding: 0.6rem;
  font-size: 1rem;
  min-height: 5rem;
}
</style>
