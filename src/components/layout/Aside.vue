<template>
  <div class="c-aside">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'CAside',
}
</script>

<style scoped lang="scss">
@include b(c-aside) {
  overflow: auto;
  flex-shrink: 0; // 防止被压缩
}
</style>
