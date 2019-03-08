<template>
  <button @click="emitEventTest">Button</button>
</template>

<script>
export default {
  name: 'myButton',
  methods: {
    emitEventTest () {
      this.$emit('event-test',event)
    }
  }
}
</script>

<style lang="scss" scoped>
button {
  margin-top: 20px;
  border: none;
  outline: none;
  background: #777;
  color: #fff;
  font-size: 12px;
  line-height: 1;
  font-weight: bold;
  display: inline-block;
  padding: 6px 8px;
  border-radius: 2px;
  &:hover {
    cursor: pointer;
  }
}
</style>
