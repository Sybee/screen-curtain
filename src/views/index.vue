<template>
  <div class="h-100">
    <div class="wrapper" >
      <ScreenCurtain />
    </div>
  </div>
</template>

<script>
import ScreenCurtain from '@/components/ScreenCurtain'

export default {
  components: { ScreenCurtain }
};
</script>

<style lang="scss">
.wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  margin: 0 auto;
}
</style>