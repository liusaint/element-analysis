<template>
  <div class="hello" :class="{ 'on': value}" @click="handleChange">11111111111111111</div>
</template>

<script>
export default {
  name: 'model',
  data () {
    return {     
      value:true
    }
  },
  methods: {
    handleChange(event) {
      this.value = !this.value;
      this.$emit('input', this.value);
      this.$emit('change', this.value);
    },
  }



}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello{
  background: #ccc;
  height: 30px;
  width: 30px;
}
.hello.on{
  background: #999;
}
</style>