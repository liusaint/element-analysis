<template>
  <div class="hello"  :class="{ 'on': value}" @click="handleChange">一个自定义表单组件，状态为：{{this.value?'on':'off'}}</div>
</template>

<script>
// 一个简单的自已定义表单组件.
export default {
  name: 'testModel',
  props: {
    value: Boolean
  },

  methods: {
    handleChange(event) { 
      // 不要直接在自定义表单组件中修改props。而是触发事件把新的值传递给父组件。父组件修改了prop,再影响到子组件。
      // this.value = !this.vaulue;    
      this.$emit('input', !this.value);
      // this.$emit('change', !this.value);
    },
  }
}
</script>


<style scoped>
.hello{
  background: #000;
  color: #fff;
  display: inline-block;
  height: 30px;
  line-height: 30px;
  padding: 0 10px;
}
.hello.on{
  background: #1388ff;
}
</style>