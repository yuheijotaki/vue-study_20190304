<template>
  <section>
    <p>{{ msg }}</p>
    <myButton @event-test="clickAlert"></myButton>
  </section>
</template>

<script>
import myButton from './../../components/element/button/myButton.vue';

export default {
  name: 'about',
  components: {
    myButton
  },
  data () {
    return {
      msg: 'Aboutページです。'
    }
  },
  methods: {
    clickAlert: function(event) {
      alert('event test');
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
