<template>
  <!-- 自定义input类型 v-model-->
  <div class="el-radio-group">
    <slot></slot>
  </div>
</template>
<script>
  import Emitter from 'element-ui/src/mixins/emitter';

  export default {
    name: 'ElRadioGroup',

    componentName: 'ElRadioGroup',

    mixins: [Emitter], // 注意这里的mixins的用法

    props: {
      value: {},
      size: String,
      fill: String,
      textColor: String,
      disabled: Boolean
    },
    watch: {
      value(value) {
        // 组件本身的change事件
        this.$emit('change', value);
        // 父组件的change事件。不冒泡？
        this.dispatch('ElFormItem', 'el.form.change', [this.value]);
      }
    }
  };
</script>

