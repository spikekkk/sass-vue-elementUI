<template>
  <transition name="slide-fade">
    <div
      v-show="isVisible"
      ref="sideModal"
      class="side-zhezhao"
    >
      <div
        v-show="isVisible"
        class="side-dialog"
      >
        <div class="contBox">
          <iframe
            :src="url"
            class="cont"
          />
        </div>
      </div>
    </div>
  </transition>
</template>
<script>
export default {
  props: {
    'visible': {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      visible2: false,
      visible3: false,
      isVisible: this.visible,
      url: ''
    }
  },
  watch: {
    'visible'(val) {
      this.isVisible = val
    },
    'isVisible'(val) {
      this.$emit('update:visible', val)
    }
  },
  mounted() {
    const side = this.$refs.sideModal
    document.body.appendChild(side)
    this.url = `${process.env.CONTENT_PATH}/micActivity/index.html?moduleId=${this.$parent.obj.activityId || ''}&moduleCode=&moduleInstId=${this.$parent.obj.id || ''}&orgId=${this.$parent.obj.orgId || ''}&orgName=${encodeURI(encodeURI(this.$store.getters.orgName || ''))}`
    const self = this
    window.addEventListener('message', function(e) {
      if (e.data === 'close') {
        self.close()
      }
    }, false)
  },
  methods: {
    close() {
      this.isVisible = false
      this.visible2 = false
    }
  }
}
</script>
<style lang="scss" scoped>
.side-zhezhao {
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0px;
  right: 0px;
  z-index: 10000;
}

.side-dialog {
  top: 0px;
  right: 0px;
  z-index: 1000;
  width: calc(100% - 150px);
  position: fixed;
  margin-left: -20px;
  height: 100%;
  background-color: rgba(255, 255, 255, 1);
  -moz-box-shadow: 2px 2px 5px #333333;
  -webkit-box-shadow: 2px 2px 5px #333333;
  box-shadow: 2px 2px 5px #333333;
  .contBox {
    width: calc(100vw - 150px);
    height: 100vh;
    .cont {
      width: 100%;
      height: 100%;
    }
  }
}

.slide-fade-enter-active {
  transition: all 0.5s ease;
}
.slide-fade-leave-active {
  transition: all 0.5s ease;
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(1000px);
  opacity: 0;
}
</style>
<style lang="scss" >
.tanchukuang {
  z-index: 20000 !important;
  p {
    font-size: 12px;
    color: #666;
  }
}
</style>

