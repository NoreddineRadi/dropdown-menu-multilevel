<template>
  <div class="bp-dropdown" :class="{ className, 'bp-dropdown--sub': role }">
    <span
      :class="{
        [`bp-dropdown__${role ? 'sub' : 'btn'}`]: true,
        [`bp-dropdown__${role ? 'sub' : 'btn'}--active`]: !isHidden,
        [`${className}-bp__btn`]: className,
        [`${className}-bp__btn--active`]: !isHidden,
      }"
      @click="_onToggle"
      @mouseenter="_onBtnEnter"
      @mouseleave="_onBtnLeave"
    >
      <slot name="btn"></slot>
      <slot name="icon" v-if="isIcon">
        <svg
          v-if="isLoading"
          class="bp-dropdown__icon bp-dropdown__icon--spin"
          viewBox="0 0 512 512"
        >
          <path
            fill="currentColor"
            d="M304 48c0 26.51-21.49 48-48 48s-48-21.49-48-48 21.49-48 48-48 48 21.49 48 48zm-48 368c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.49-48-48-48zm208-208c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.49-48-48-48zM96 256c0-26.51-21.49-48-48-48S0 229.49 0 256s21.49 48 48 48 48-21.49 48-48zm12.922 99.078c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48c0-26.509-21.491-48-48-48zm294.156 0c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48c0-26.509-21.49-48-48-48zM108.922 60.922c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.491-48-48-48z"
          ></path>
        </svg>
        <svg
          v-else
          class="bp-dropdown__icon"
          :class="{ [`bp-dropdown__icon--${align}`]: align }"
          viewBox="0 0 256 512"
        >
          <path
            fill="currentColor"
            d="M119.5 326.9L3.5 209.1c-4.7-4.7-4.7-12.3 0-17l7.1-7.1c4.7-4.7 12.3-4.7 17 0L128 287.3l100.4-102.2c4.7-4.7 12.3-4.7 17 0l7.1 7.1c4.7 4.7 4.7 12.3 0 17L136.5 327c-4.7 4.6-12.3 4.6-17-.1z"
          ></path>
        </svg>
      </slot>
    </span>
    <transition name="fade">
      <div
        v-if="!isHidden"
        class="bp-dropdown__body"
        :id="id"
        :style="{ minWidth: `${width}px`, top: `${top}px`, left: `${left}px` }"
        :class="{ [`${className}-bp__body`]: className }"
        @click="_onBodyClick"
        @mouseenter="_onBodyEnter"
        @mouseleave="_onBodyLeave"
      >
        <slot name="body"></slot>
      </div>
    </transition>
  </div>
</template>

<script>
// eslint-disable-entire-file no-console
/* eslint-disable no-debugger, no-console */
export default {
  name: "bp-vuejs-dropdown",

  props: {
    role: {
      type: String,
      default: "",
    },
    asContextMenu: {
      type: Boolean,
      default: false
    },
    unscroll: {
      type: [HTMLElement, String],
      default: null,
    },

    align: {
      type: String,
      default: "right",
    },
    x: {
      type: Number,
      default: 0,
    },

    y: {
      type: Number,
      default: 0,
    },

    beforeOpen: {
      type: Function,
      default: (resolve) => resolve(),
    },

    trigger: {
      type: String,
      default: "click",
    },

    closeOnClick: {
      type: Boolean,
      required: false,
      default: false,
    },

    isIcon: {
      type: Boolean,
      required: false,
      default: true,
    },

    className: {
      type: String,
      required: false,
      default: "",
    },
  },

  data() {
    return {
      isHidden: true,
      isLoading: false,
      id: null,
      timeout: null,
      top: undefined,
      right: undefined,
      bottom: undefined,
      left: undefined,
      width: undefined,
    };
  },

  watch: {
    isHidden(isHidden) {
      if (this.unscroll) {
        const el =
          this.unscroll instanceof HTMLElement
            ? this.unscroll
            : document.querySelector(this.unscroll);

        if (el) {
          el.style.overflow = !isHidden ? "hidden" : "";
        }
      }
    },
    left(nVal) {
      if(this.asContextMenu) {
        const screenWidth = document.documentElement.clientWidth || window.innerWidth
        const body = document.getElementById(this.id);
        if (body.offsetWidth + nVal > screenWidth) {
          this.left = screenWidth-body.offsetWidth
        }
      }
    },
    top(nVal) {
      if(this.asContextMenu) {
        const screenHeight = document.documentElement.clientHeight || window.innerHeight
        const body = document.getElementById(this.id);
        if (body.offsetHeight + nVal > screenHeight) {
          this.top = nVal - body.offsetHeight
        }
      }
    }
  },

  created() {
    const $root = this.$root;
    // $root.$on("bp-dropdown:open", () => (this.isHidden = true));
    // $root.$on("bp-dropdown:hide", () => (this.isHidden = true));
    if (this.trigger === "click" && !$root["is-bp-dropdown"]) {
      Object.defineProperty($root, "is-bp-dropdown", {
        enumerable: false,
        configurable: false,
        writable: false,
        value: true,
      });

      document.onmousedown = (e) => {
        const target = e.target;
        const dropdown =
          target.closest(".bp-dropdown__btn") ||
          target.closest(".bp-dropdown__body");

        if (!dropdown) {
          $root.$emit("bp-dropdown:hide");
        }
      };
    }

    this.id = "bp-dropdown-" + this.generateRandomId();
  },

  methods: {
    generateRandomId() {
      return Math.random()
        .toString(36)
        .substr(2, 10);
    },
    _onToggle(e) {
      if (this.trigger !== "click") {
        return;
      }
      this.checkCustomCallback(e);
    },
    _onBtnEnter(e) {
      if (this.trigger !== "hover" || !this.isHidden) {
        return;
      }
      this.checkCustomCallback(e);
    },
    _onBtnLeave(e) {
      if (this.trigger !== "hover") {
        return;
      }

      if (this.role) {
        this.timeout = setTimeout(() => (this.isHidden = true), 50);
      }
      const to = e.toElement;
      if (!to) {
        return;
      }
      const isDropdown =
        to.closest(".bp-dropdown__btn") || to.closest(".bp-dropdown__body");
      if (isDropdown) {
        return;
      }
      this.prepare();
    },
    _onBodyClick() {
      if (this.closeOnClick) {
        this.isHidden = true;
      }
    },
    _onBodyEnter() {
      if (this.timeout) {
        clearTimeout(this.timeout);
      }
    },
    _onBodyLeave(e) {
      if (this.trigger !== "hover") {
        return;
      }
      const to = e.toElement;
      if (!to) {
        return;
      }
      if (to.closest(".bp-dropdown__btn") || to.closest(".bp-dropdown__sub")) {
        return;
      }
      this.prepare();
    },
    checkCustomCallback(e) {
      if (!this.isHidden) {
        this.prepare();
        return;
      }
      const promise = new Promise((resolve) => {
        this.isLoading = true;
        this.beforeOpen.call(this, resolve);
      });
      promise.then(() => {
        this.isLoading = false;
        if (!e.target.closest(".bp-dropdown__body")) {
          this.$root.$emit("bp-dropdown:open");
        }
        setTimeout(this.prepare, 0);
      });
      promise.catch(() => {
        throw Error("bp-dropdown promise error");
      });
    },
    prepare() {
      this.isHidden = !this.isHidden;
      this.$emit("clickOnItem", this.isHidden);
      if (!this.isHidden) {
        this.$nextTick(() => {
          let parent;
          const button = this.$el.firstElementChild;
          if (this.$el.offsetParent.className.includes("bp-dropdown")) {
            parent = this.$el.offsetParent;
          } else {
            parent = this.$el.firstChild;
          }
          const container = document.getElementById(this.id);
          this.setWidth(button.offsetWidth);
          this.setPosition(button, container, parent);
        });
      }
    },
    setWidth(width) {
      this.width = width;
    },
    setPosition(btn, body, parent) {
      if (!parent || !btn || !body) {
        return;
      }
      const coords = this.getCoords(parent);
      const btnCoords = this.getCoords(btn);
      const currentTop = btnCoords.top;
      const currentLeft = coords.left;
      const btnWidth = parent.offsetWidth;
      const btnHeight = btn.offsetHeight;

      const bodyWidth = body.offsetWidth;
      const bodyHeight = body.offsetHeight;
      let bodyPosition;
      const btnGeometrics = btn.getBoundingClientRect();
      const bodyGeometrics = body.getBoundingClientRect();
      const bodyIsTottalyShowY = btnGeometrics.bottom + bodyGeometrics.height + this.y <= (document.documentElement.clientHeight || window.innerHeight)
      const bodyIsTottalyShowX = btnGeometrics.left + btnGeometrics.width + bodyGeometrics.width + this.x <= (document.documentElement.clientWidth || window.innerWidth)
      
      if (
        this.align === 'right' &&
        btnGeometrics.right + bodyGeometrics.width + this.x >
        (document.documentElement.clientWidth || window.innerWidth)
      ) {
        bodyPosition = "left";
      } else if (this.align === 'left' && btnGeometrics.left - bodyGeometrics.width + this.x <= 0) {
        bodyPosition = "right";
      } else if (this.align === 'top' && btnGeometrics.top - bodyGeometrics.height + this.y <= 0) {
        bodyPosition = "bottom";
      } else if (this.align === 'bottom' && !bodyIsTottalyShowY) {
        bodyPosition = "top";
      } 
      else {

        bodyPosition = this.align
      }
      switch (bodyPosition) {
        case "top":
          this.top = bodyIsTottalyShowY ? currentTop + pageYOffset : currentTop + pageYOffset - bodyHeight;
          this.left = bodyIsTottalyShowX ? currentLeft + pageXOffset :  currentLeft + pageXOffset - ( currentLeft > 0 && this.align !== bodyPosition ? bodyWidth : 0) +  (currentLeft > 0 && this.align !== bodyPosition ? btnWidth : 0)
          break;
        case "right":
          this.top = bodyIsTottalyShowY ? currentTop + pageYOffset : currentTop + pageYOffset - bodyHeight + btnHeight;
          this.left = bodyIsTottalyShowX ? currentLeft + pageXOffset + btnWidth : currentLeft + pageXOffset + btnWidth
          break;
        case "bottom":
          this.top = bodyIsTottalyShowY ? currentTop + pageYOffset+ btnHeight : currentTop + pageYOffset - bodyHeight;
          this.left = bodyIsTottalyShowX ? currentLeft + pageXOffset : currentLeft + pageXOffset - ( currentLeft > 0 && this.align === bodyPosition ? bodyWidth : 0) +  (currentLeft > 0 && this.align === bodyPosition ? btnWidth : 0);
          break;
        case "left":
          this.top = bodyIsTottalyShowY ? currentTop + pageYOffset : currentTop + pageYOffset - bodyHeight + btnHeight;
          this.left = bodyIsTottalyShowX ? currentLeft + pageXOffset - btnWidth: currentLeft + pageXOffset - bodyWidth;
          break;
        default:
          this.top = currentTop + pageYOffset + btnHeight;
          this.left = currentLeft + pageXOffset;
          break;
      }
      this.top += this.y;
      this.left += this.x;
    },
    getCoords(el) {
      el = el.getBoundingClientRect();
      return {
        top: el.top - pageYOffset,
        left: el.left - pageXOffset,
      };
    },
  },
};
</script>

<style>
.bp-dropdown--sub {
  width: 100%;
}

.bp-dropdown--sub .bp-dropdown__btn,
.bp-dropdown--sub .bp-dropdown__sub {
  width: 100%;
}

.bp-dropdown--sub .bp-dropdown__icon {
  margin-left: auto;
}

.bp-dropdown__btn {
  display: inline-flex;
  align-items: center;
  padding: 3px 5px;
  border: 1px solid #efefef;
  cursor: pointer;
  transition: background-color 0.1s ease;
}

.bp-dropdown__sub {
  display: inline-flex;
  align-items: center;
}

.bp-dropdown__btn--active {
  background-color: #eee;
}

.bp-dropdown__icon {
  display: inline-block;
  width: 15px;
  height: 15px;
  overflow: visible;
  transition: transform 0.1s ease;
}

.bp-dropdown__icon--spin {
  width: 12px;
  height: 12px;
  animation: spin 2s infinite linear;
}

.bp-dropdown__icon--top {
  transform: rotate(-180deg);
}

.bp-dropdown__icon--right {
  transform: rotate(-90deg);
}

.bp-dropdown__icon--bottom {
  transform: rotate(0);
}

.bp-dropdown__icon--left {
  transform: rotate(-270deg);
}

.bp-dropdown__btn--active .bp-dropdown__icon--top,
.bp-dropdown__sub--active .bp-dropdown__icon--top {
  transform: rotate(0);
}

.bp-dropdown__btn--active .bp-dropdown__icon--right,
.bp-dropdown__sub--active .bp-dropdown__icon--right {
  transform: rotate(-270deg);
}

.bp-dropdown__btn--active .bp-dropdown__icon--bottom,
.bp-dropdown__sub--active .bp-dropdown__icon--bottom {
  transform: rotate(-180deg);
}

.bp-dropdown__btn--active .bp-dropdown__icon--left,
.bp-dropdown__sub--active .bp-dropdown__icon--left {
  transform: rotate(-90deg);
}

.bp-dropdown__body {
  position: fixed;
  top: 0;
  left: 0;
  padding: 6px 8px;
  background-color: #fff;
  box-shadow: 0 5px 15px -5px rgba(0, 0, 0, 0.5);
  z-index: 9999;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.1s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

@keyframes spin {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
