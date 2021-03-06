<template>
  <v-popover
    ref="popover"
    class="tooltip"
    :popover-class="[negative ? 'negative' : '']"
    :auto-hide="true"
    :placement="placement"
    trigger="manual"
    delay.show="100"
    delay.hide="3000"
  >
    <span @click="click" @mouseleave="mouseLeave" @mouseover="mouseOver">
      <slot />
    </span>
    <template slot="popover">
      {{ content }}
    </template>
  </v-popover>
</template>

<script>
import { VPopover } from 'v-tooltip'

export default {
  components: {
    VPopover
  },
  props: {
    trigger: {
      type: String,
      default: 'hover'
    },
    content: {
      type: String,
      default: ''
    },
    negative: {
      type: Boolean,
      default: false
    },
    placement: {
      type: String,
      default: 'auto',
      validator: pos => ['auto', 'top', 'right', 'bottom', 'left'].includes(pos)
    }
  },

  methods: {
    show() {
      this.$refs.popover.show()
    },

    hide() {
      this.$refs.popover.hide()
    },

    mouseLeave() {
      this.hide()
    },

    mouseOver() {
      if (!this.content) {
        return
      }

      if (this.trigger !== 'hover') {
        return
      }

      this.show()
    },

    click() {
      if (!this.content) {
        return
      }

      if (this.trigger !== 'click') {
        return
      }

      this.show()
      this.$emit('click')
      setTimeout(() => this.hide(), 6000)
    }
  }
}
</script>

<style lang="scss">
@import 'boot';
@import 'tokens/color';
@import 'font';

.tooltip {
  z-index: 10000;

  .tooltip-inner {
    background: $background-color;
    color: $overlay-color;
    border-radius: 3px;
    padding: 5px 10px 4px;
    text-align: center;
  }

  .tooltip-arrow {
    width: 0;
    height: 0;
    border-style: solid;
    position: absolute;
    margin: 5px;
    border-color: $overlay-color;
    z-index: 1;
  }

  &[x-placement^='top'] {
    margin-bottom: 8px;

    .tooltip-arrow {
      border-width: 3px 3px 0 3px;
      border-left-color: transparent !important;
      border-right-color: transparent !important;
      border-bottom-color: transparent !important;
      bottom: -3px;
      left: calc(50% - 5px);
      margin-top: 0;
      margin-bottom: 0;
    }
  }

  &[x-placement^='bottom'] {
    margin-top: 5px;

    .tooltip-arrow {
      border-width: 0 5px 5px 5px;
      border-left-color: transparent !important;
      border-right-color: transparent !important;
      border-top-color: transparent !important;
      top: -5px;
      left: calc(50% - 5px);
      margin-top: 0;
      margin-bottom: 0;
    }
  }

  &[x-placement^='right'] {
    margin-left: 5px;

    .tooltip-arrow {
      border-width: 5px 5px 5px 0;
      border-left-color: transparent !important;
      border-top-color: transparent !important;
      border-bottom-color: transparent !important;
      left: -5px;
      top: calc(50% - 5px);
      margin-left: 0;
      margin-right: 0;
    }
  }

  &[x-placement^='left'] {
    margin-right: 5px;

    .tooltip-arrow {
      border-width: 5px 0 5px 5px;
      border-top-color: transparent !important;
      border-right-color: transparent !important;
      border-bottom-color: transparent !important;
      right: -5px;
      top: calc(50% - 5px);
      margin-left: 0;
      margin-right: 0;
    }
  }

  &.negative {
    .tooltip-inner {
      background: $overlay-color;
      color: $background-color;
    }

    .tooltip-arrow {
      border-color: $overlay-color;
    }
  }
}
</style>
