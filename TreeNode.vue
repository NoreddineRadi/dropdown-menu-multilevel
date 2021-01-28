<template>
  <dropdown
    :role="role"
    :align="align"
    :x="offsetX"
    :y="offsetY"
    :trigger="trigger"
    :isIcon="!hideIcon"
    :class-name="className"
    :ref="id"
  >
    <template slot="btn" class="cursor-pointer d-flex">
      <slot v-if="$slots.btn" name="btn" />
      <div v-else @click="clickOnButton(id)" class="flex-fill">
        {{ label }}
      </div>
    </template>
    <template slot="body">
      <slot v-if="$slots.body" name="body" />
      <div v-else>
        <div v-for="(el, index) in list" :key="el+index">
          <div class="text-left d-flex align-items-center cursor-pointer p-1">
            <span class="material-icons mr-2">{{ el.icon }}</span>
            <TreeNode
              :id="el.label"
              v-if="el.children.length"
              :list="el.children"
              :label="el.label"
              :offsetX="el.offsetX || offsetX"
              :offsetY="el.offsetY || offsetY"
              :align="`${el.align || align}`"
              :hideIcon="el.hideIcon || false"
              :className="el.className || className"
              :trigger="trigger"
              @clickOnButton="clickOnButton(id)"
              @clickOnItem="clickOnItem($event)"
            />
            <div :class="className" v-else @click="clickOnItem(el.label)">
              {{ el.label }}
            </div>
          </div>
        </div>
      </div>
    </template>
  </dropdown>
</template>

<script>
import dropdown from 'bp-vuejs-dropdown'
export default {
  name: 'TreeNode',
  components: {
    dropdown
  },
  props: {
    list: {
      type: Array,
      default: () => [],
    },
    id: {
      type: String,
      default: 'main-dropdown',
    },
    role: {
      type: String,
      default: 'sublist',
    },
    label: {
      type: String,
      default: 'dropDown',
    },
    align: {
      type: String,
      default: '',
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
      default: 'click',
    },
    hideIcon: {
      type: Boolean,
      default: false,
    },
    className: {
      type: String,
      default: 'class-name',
    },
  },
  methods: {
    beforOpen(resolve) {
      this.$emit('beforOpen', resolve);
    },
    clickOnItem(el) {
      const main = this.$refs['main-dropdown'];
      main && (main.isHidden = true);
      this.$emit('clickOnItem', el);
    },
    clickOnButton(id) {
      this.$parent.$children.forEach(el => {
        if (el.id !== id) {
          const comp = el.$refs[el.id]
          if (!comp.isHidden) {
            comp.isHidden = true
          }
        }
      })
      this.$emit('clickOnButton')
    },
    
  },
};
</script>
<style lang="scss">
.cursor-pointer {
  cursor: pointer;
}
</style>
