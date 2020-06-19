<template>
  <transition name="sl-message-fade"
              @after-leave="handleAfterLeave">
    <div :class="[
        'sl-message',
        type && !iconClass ? `sl-message--${ type }` : '',
        center ? 'is-center' : '',
        showClose ? 'is-closable' : '',
        customClass
      ]"
         :style="positionStyle"
         v-show="visible"
         @mouseenter="clearTimer"
         @mouseleave="startTimer"
         role="alert">
      <i :class="iconClass"
         v-if="iconClass"></i>
      <i :class="typeClass"
         v-else></i>
      <slot>
        <p v-if="!dangerouslyUseHTMLString"
           class="sl-message__content">{{ message }}</p>
        <p v-else
           v-html="message"
           class="sl-message__content"></p>
      </slot>
      <i v-if="showClose"
         class="sl-message__closeBtn sl-icon-close"
         @click="close"></i>
    </div>
  </transition>
</template>

<script type="text/babel">
const typeMap = {
  success: 'success',
  info: 'info',
  warning: 'warning',
  error: 'error'
};

export default {
  data () {
    return {
      visible: false,
      message: '',
      duration: 3000,
      type: 'info',
      iconClass: '',
      customClass: '',
      onClose: null,
      showClose: false,
      closed: false,
      verticalOffset: 20,
      timer: null,
      dangerouslyUseHTMLString: false,
      center: false
    };
  },

  computed: {
    typeClass () {
      return this.type && !this.iconClass
        ? `sl-message__icon sl-icon-${typeMap[this.type]}`
        : '';
    },
    positionStyle () {
      return {
        'top': `${this.verticalOffset}px`
      };
    }
  },

  watch: {
    closed (newVal) {
      if (newVal) {
        this.visible = false;
      }
    }
  },

  methods: {
    handleAfterLeave () {
      this.$destroy(true);
      this.$el.parentNode.removeChild(this.$el);
    },

    close () {
      this.closed = true;
      if (typeof this.onClose === 'function') {
        this.onClose(this);
      }
    },

    clearTimer () {
      clearTimeout(this.timer);
    },

    startTimer () {
      if (this.duration > 0) {
        this.timer = setTimeout(() => {
          if (!this.closed) {
            this.close();
          }
        }, this.duration);
      }
    },
    keydown (e) {
      if (e.keyCode === 27) { // esc关闭消息
        if (!this.closed) {
          this.close();
        }
      }
    }
  },
  mounted () {
    this.startTimer();
    document.addEventListener('keydown', this.keydown);
  },
  beforeDestroy () {
    document.removeEventListener('keydown', this.keydown);
  }
};
</script>

<style lang="scss" scoped>
.sl-message {
  min-width: 380px;
  box-sizing: border-box;
  border-radius: 4px;
  border-width: 1px;
  border-style: solid;
  border-color: #4662d9;
  position: fixed;
  left: 50%;
  top: 20px;
  transform: translateX(-50%);
  background-color: #edf2fc;
  transition: opacity 0.3s, transform 0.4s, top 0.4s;
  overflow: hidden;
  padding: 15px 15px 15px 20px;
  display: flex;
  align-items: center;
}

p {
  margin: 0;
}

.sl-message__content {
  padding-right: 16px;
}

.sl-message-fade-enter,
.sl-message-fade-leave-active {
  opacity: 0;
  transform: translate(-50%, -100%);
}
</style>
