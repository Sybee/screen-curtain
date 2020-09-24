<template>
  <div ref="curtain" class="card border-0 bg-transparent curtain" :class="{ 'curtain--animated': animated }">
    <button ref="grip" class="grip btn btn-danger btn-sm btn-block" type="button">
      <i class="material-icons">drag_handle</i>
    </button>
    <div ref="scrollbox" class="scrollbox" :class="{ 'scrollbox--scrollable': !scrollboxDragging }">
      <ul class="list-group">
        <li class="list-group-item bg-secondary rounded-0 text-light" v-for="(item, index) in 30" :key="index">List item #{{ item }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import { debounce } from 'lodash'

export default {
  data: () => ({
    animated: false,
    collapsed: true,
    scrollboxDragging: false,
    scrolledToTop: true,
    scrolledToBottom: false,
    touchStartEvent: null,
    gripSwipeThreshold: 150, // px
    scrollboxSwipeThreshold: 50, // px
  }),
  methods: {

  },
  mounted() {
    this.$refs.grip.addEventListener('touchstart', (e) => {
      this.animated = false
      this.touchStartEvent = e
    })

    this.$refs.grip.addEventListener('touchmove', (e) => {
      if (e.touches[0].pageY >= 0) {
        // Если убрать этот if, то при свайпе шторки вверх тоже будет инерция, на твое усмотрение. Есть 1 декоративный косяк.
        this.$refs.curtain.style.top = e.touches[0].clientY + 'px'
      }
    })

    this.$refs.grip.addEventListener('touchend', (e) => {
      this.animated = true

      if (e.changedTouches[0].pageY > this.touchStartEvent.touches[0].pageY) {
        // Свайп был сверху-вниз
        if (e.changedTouches[0].pageY >= this.gripSwipeThreshold) {
          this.$refs.curtain.style.top = this.$refs.scrollbox.offsetHeight + 'px'
        } else {
          this.$refs.curtain.style.top = 0
        }
      } else {
        // Свайп был снизу-вверх
        if (e.changedTouches[0].pageY <= (window.innerHeight - this.gripSwipeThreshold)) {
          this.$refs.curtain.style.top = 0
        } else {
          this.$refs.curtain.style.top = this.$refs.scrollbox.offsetHeight + 'px'
        }
      }
    })

    let start = null

    this.$refs.scrollbox.addEventListener('touchstart', (startEvent) => {
      this.animated = false
      start = startEvent 
    })

    this.$refs.scrollbox.addEventListener('touchmove', (moveEvent) => {
      if (this.$refs.scrollbox.scrollTop === 0) {
        if (moveEvent.touches[0].pageY >= (start.touches[0].pageY + this.scrollboxSwipeThreshold)) {
          this.$refs.curtain.style.top = (moveEvent.touches[0].pageY - this.$refs.grip.offsetHeight - this.scrollboxSwipeThreshold) + 'px'
          this.scrollboxDragging = true
        } 
      }
    })

    this.$refs.scrollbox.addEventListener('touchend', (e) => {
      if (this.scrollboxDragging) {
        this.animated = true

        if (e.changedTouches[0].pageY >= this.gripSwipeThreshold) {
          this.$refs.curtain.style.top = this.$refs.scrollbox.offsetHeight + 'px'
        } else {
          this.$refs.curtain.style.top = 0
        }

        this.scrollboxDragging = false
      }
    })
  }
}
</script>

<style lang="scss" scoped>
.curtain {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  width: 100%;
  height: 100%;

  &--animated {
    transition: top 180ms ease-out;
  }
  
}

.grip {
  height: 40px;
  cursor: grab;

  &:hover { cursor: grab; }
  &:active { cursor: grabbing; }
}

.scrollbox {
  position: absolute;
  left: 0;
  right: 0;
  top: 40px;
  bottom: 0;
  max-height: 100%;
  overflow-x: hidden;
  overflow-y: hidden;

  &--scrollable {
    overflow-y: scroll;
  }
}
</style>