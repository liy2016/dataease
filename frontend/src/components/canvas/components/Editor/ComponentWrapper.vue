<template>
  <div
    :style="getOutStyleDefault(config.style)"
    :class="{'gap_class':canvasStyleData.panel.gap==='yes'}"
    class="component"
    @click="handleClick"
  >
    <de-out-widget
      v-if="config.type==='custom'"
      :id="'component' + config.id"
      class="component-custom"
      :style="getComponentStyleDefault(config.style)"
      :out-style="config.style"
      :element="config"
      :in-screen="inScreen"
    />

    <component
      :is="config.component"
      v-else
      :out-style="config.style"
      :style="getComponentStyleDefault(config.style)"
      :prop-value="config.propValue"
      :element="config"
      :search-count="searchCount"
    />
  </div>
</template>

<script>
import { getStyle } from '@/components/canvas/utils/style'
import runAnimation from '@/components/canvas/utils/runAnimation'
import { mixins } from '@/components/canvas/utils/events'
import { mapState } from 'vuex'
import DeOutWidget from '@/components/dataease/DeOutWidget'
export default {
  components: { DeOutWidget },
  mixins: [mixins],
  props: {
    config: {
      type: Object,
      require: true,
      default: null
    },
    filter: {
      type: Object,
      require: false,
      default: null
    },
    searchCount: {
      type: Number,
      required: false,
      default: 0
    },
    inScreen: {
      type: Boolean,
      required: false,
      default: true
    }
  },
  computed: {
    ...mapState([
      'canvasStyleData'
    ])
  },
  mounted() {
    runAnimation(this.$el, this.config.animations)
  },
  methods: {
    getStyle,

    getOutStyleDefault(style) {
      const result = {};
      ['width', 'left'].forEach(attr => {
        result[attr] = style[attr] + 'px'
      });
      ['height', 'top'].forEach(attr => {
        result[attr] = style[attr] + 'px'
      })
      result['rotate'] = style['rotate']
      result['opacity'] = style['opacity']

      return result
      // return style
    },

    getComponentStyleDefault(style) {
      return getStyle(style, ['top', 'left', 'width', 'height', 'rotate'])
      // return style
    },

    handleClick() {
      const events = this.config.events
      Object.keys(events).forEach(event => {
        this[event](events[event])
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.component {
    position: absolute;
}

.gap_class{
  padding:3px;
}
.component-custom {
  outline: none;
  width: 100% !important;
  height: 100%;
}
</style>
