<template>
  <div class="cntr">
    <input
      v-on="$listeners"
      class="inp-cbx"
      :id="'checkbox-' + this._uid"
      type="checkbox"
      v-model="checkedData"
    />
    <label
      @mouseover="hover"
      @mouseleave="hoverBorder = ''"
      class="cbx"
      :for="'checkbox-' + this._uid"
      ><span class="checkbox" :class="hoverBorder + ' ' + checkedBg">
        <svg width="12px" height="10px">
          <use xlink:href="#check"></use></svg></span
      ><span class="checkbox-name" :class="'text-' + theme"><slot /></span
    ></label>
    <!--SVG Sprites-->
    <svg class="inline-svg">
      <symbol id="check" viewbox="0 0 12 10">
        <polyline points="1.5 6 4.5 9 10.5 1"></polyline>
      </symbol>
    </svg>
  </div>
</template>
<script>
export default {
  data () {
    return {
      hoverBorder: '',
      checkedBg: '',
      checkedData: this.value
    }
  },
  created () {
    if (this.value) this.updateVariant()
  },
  methods: {
    hover () {
      this.hoverBorder = 'border border-' + this.variant
    },
    updateVariant () {
      this.checkedBg = `border border-${this.variant} bg-` + this.variant
    }
  },
  watch: {
    value () {
      this.checkedData = this.value
    },
    checkedData () {
      if (this.checkedData) {
        this.$emit('input', true)
        this.updateVariant()
      } else {
        this.$emit('input', false)
        this.checkedBg = ''
      }
    }
  },
  props: {
    variant: {
      type: String,
      default: 'primary'
    },
    theme: {
      type: String,
      default: 'dark'
    },
    value: {
      default: false
    }
  }
}
</script>
<style scoped>
* {
  box-sizing: border-box;
}
.cbx {
  -webkit-user-select: none;
  user-select: none;
  cursor: pointer;
  padding: 6px 8px;
  border-radius: 6px;
  overflow: hidden;
  transition: all 0.2s ease;
}
.cbx:not(:last-child) {
  margin-right: 6px;
}
/* .cbx:hover {
  background: rgba(0, 119, 255, 0.06);
} */
.cbx span {
  float: left;
  vertical-align: middle;
  transform: translate3d(0, 0, 0);
}
.cbx span:first-child {
  position: relative;
  width: 18px;
  height: 18px;
  border-radius: 4px;
  transform: scale(1);
  border: 1px solid #cccfdb;
  transition: all 0.2s ease;
  box-shadow: 0 1px 1px rgba(0, 16, 75, 0.05);
}
.cbx span:first-child svg {
  position: absolute;
  top: 3px;
  left: 2px;
  fill: none;
  stroke: #fff;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 16px;
  stroke-dashoffset: 16px;
  transition: all 0.3s ease;
  transition-delay: 0.1s;
  transform: translate3d(0, 0, 0);
}
.cbx span:last-child {
  padding-left: 8px;
  line-height: 18px;
}
.inp-cbx {
  position: absolute;
  visibility: hidden;
}
.inp-cbx:checked + .cbx span:first-child {
  /* background: #07f; */
  /* border-color: #07f; */
  animation: wave 0.4s ease;
}
.inp-cbx:checked + .cbx span:first-child svg {
  stroke-dashoffset: 0;
}

.inline-svg {
  position: absolute;
  width: 0;
  height: 0;
  pointer-events: none;
  user-select: none;
}
@media screen and (max-width: 640px) {
  .cbx {
    width: 100%;
    margin-bottom: 4px;
    display: inline-block;
  }
}
@-moz-keyframes wave {
  50% {
    transform: scale(0.9);
  }
}
@-webkit-keyframes wave {
  50% {
    transform: scale(0.9);
  }
}
@-o-keyframes wave {
  50% {
    transform: scale(0.9);
  }
}
@keyframes wave {
  50% {
    transform: scale(0.9);
  }
}
</style>
