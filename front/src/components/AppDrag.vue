<template>
  <div
    draggable
    @dragstart.self="onDrag"
    @dragover.prevent
    @dragenter.prevent
    >
    <slot />
  </div>
</template>

<script>
export default {
  props: {
    transferData: {
      type: Object,
      required: true
    }
  },
  onDrag (e) {
    e.dataTransfer.setData('payload', JSON.stringify(this.transferData))
  }
}
</script>