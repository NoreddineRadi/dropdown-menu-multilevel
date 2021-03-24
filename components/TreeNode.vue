<template>
  <dropdown
    :role="role"
    :align="align || 'right'"
    :x="offsetX"
    :y="offsetY"
    :trigger="trigger"
    :isIcon="asContextMenu ? false : !hideChevron"
    :class-name="className"
    :ref="id"
    :asContextMenu="asContextMenu"
    @clickOnItem="clickOnButton(id)"
    v-click-outside="clickOutSide"
  >
    <template slot="btn" class="cursor-pointer d-flex">
      <template v-if="!asContextMenu">
        <slot v-if="$slots['btn-main']" name="btn-main" />
        <div v-else @click="clickOnButton(id)" class="d-flex align-items-center">
          <span class="material-icons mr-2">{{ iconName }}</span>
           <span>{{ label }}</span>
        </div>
      </template>
    </template>
    <template slot="body">
      <slot v-if="$slots.body" name="body" />
      <div v-else>
        <div v-for="(el, index) in list" :key="el + index">
          <div v-if="el.label" class="text-left d-flex align-items-center cursor-pointer p-1">
            <TreeNode
              :id="'Tree-node' + el.label"
              v-if="el.children && el.children.length"
              :list="el.children"
              :label="el.label "
              :offsetX="el.offsetX"
              :offsetY="el.offsetY"
              :align="el.align || 'right'"
              :hideChevron="el.hideChevron || false"
              :className="el.className"
              :iconName="el.icon"
              role="sublist"
              :trigger="trigger"
              @clickOnButton="clickOnButton(id)"
              @clickOnItem="clickOnItem($event)"
            />
            <div :class="el.className" v-else   @click="clickOnItem(el.label)" class="d-flex align-items-center">
              <span class="material-icons mr-2">{{ el.icon }}</span>
              <span>{{ el.label }}</span>
            </div>
          </div>
        </div>
      </div>
    </template>
  </dropdown>
</template>

<script>
// eslint-disable-entire-file no-console
/* eslint-disable no-debugger, no-console */
import dropdown from "./Dropdown.vue";
import clickOutside from './directives/clickOutside';
export default {
  name: "TreeNode",
  components: {
    dropdown,
  },
  props: {
    list: {
      type: Array,
      default: () => [],
    },
    id: {
      type: String,
      default: "Tree-node-main-dropdown",
    },
    role: {
      type: String,
      default: "sublist",
    },
    label: {
      type: String,
      validator(v) {
        return !!v
      },
      required: true,
    },
    align: {
      type: String,
      default: "right",
    },
    offsetX: {
      type: Number,
      default: 0,
    },
    offsetY: {
      type: Number,
      default: 0,
    },
    trigger: {
      type: String,
      default: "click",
    },
    hideChevron: {
      type: Boolean,
      default: false,
    },
    className: {
      type: String,
      default: "",
    },
    iconName: {
      type: String,
      default: "",
    },
    asContextMenu: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    beforOpen(resolve) {
      this.$emit('beforOpen', resolve);
    },
    clickOutSide() {
       const main = this.$refs["Tree-node-main-dropdown"];
       main && (main.isHidden = true);
    },
    clickOnItem(el) {
      const main = this.$refs['Tree-node-main-dropdown'];
      main && (main.isHidden = true);
      this.$emit('clickOnItem', el);
    },
    clickOnButton(id) {
      this.$parent.$children.forEach(el => {
        if (el.id !== id) {
          const comp = el.$refs[el.id]
          if (comp && !comp.isHidden) {
            comp.isHidden = true
          }
        }
      });
      this.$emit('clickOnButton');
    },
  },
  directives: {
    'click-outside': clickOutside,
  },
};
</script>
<style lang="scss">
.cursor-pointer {
  cursor: pointer;
}
</style>
