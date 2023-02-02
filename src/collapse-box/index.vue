<template>
  <div class="collapse-box">
    <div v-show="show" ref="wrapper" class="wrapper" @transitionend="onTransitionEnd">
      <div ref="content">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
import { doubleRaf } from '../utils/dom/raf';

export default {
  name: 'collapse-box',
  components: {
  },
  props: {
      expanded: {
          type: Boolean,
          default: false,
          isRequired: false
      }
  },
  data() {
    return {
      show: null,
      opened: null,
    };
  },
  created() {
    this.opened = this.expanded;
    this.show = this.expanded;
  },
  methods: {
    toggle() {
      this.opened = !this.opened;
      if (this.opened) {
        this.show = true;
      }

      this.$nextTick(()=> {
        const { content, wrapper } = this.$refs;
        if (!content || !wrapper) {
          return;
        }
        const { offsetHeight } = content;
        if (offsetHeight) {
          const contentHeight = `${offsetHeight}px`;
          wrapper.style.height = this.opened ? 0 : contentHeight;

          // use double raf to ensure animation can start
          doubleRaf(() => {
            wrapper.style.height = this.opened ? contentHeight : 0;
          });
        } else {
          this.onTransitionEnd();
        }
      })
    },
    onTransitionEnd() {
      if (!this.opened) {
        this.show = false;
      } else {
        this.$refs.wrapper.style.height = '';
      }
    }
  }
}
</script>

<style>
.wrapper {
  overflow: hidden;
  transition: height 0.3s ease-in-out;
  will-change: height;
}
</style>

