<template>
  <label
    class="el-radio-button"
    :class="[
      size ? 'el-radio-button--' + size : '',
      { 'is-active': value === label },
      { 'is-disabled': isDisabled }
    ]"
  >
    <input
      class="el-radio-button__orig-radio"
      :value="label"
      type="radio"
      v-model="value"
      :name="name"
      :disabled="isDisabled">
    <span class="el-radio-button__inner" :style="value === label ? activeStyle : null">
      <slot></slot>
      <!-- 直接使用:label或组件中的值主在这里 -->
      <template v-if="!$slots.default">{{label}}</template>
    </span>
  </label>
</template>
<script>
  export default {
    name: 'ElRadioButton',

    props: {
      label: {},
      disabled: Boolean,
      name: String
    },
    computed: {
      value: {
        get() {
          // 父级的value。父级获取到值之后再分发给子组件。
          return this._radioGroup.value;
        },
        set(value) {
          // 触发父级的input事件。把value值传给父级。
          this._radioGroup.$emit('input', value);
        }
      },
      // 往上级找，一直到上级的组件名为ElRadioGroup;
      _radioGroup() {
        let parent = this.$parent;
        while (parent) {
          if (parent.$options.componentName !== 'ElRadioGroup') {
            parent = parent.$parent;
          } else {
            return parent;
          }
        }
        return false;
      },
      // 从上层组件拿到对应的样式什么的。
      activeStyle() {
        return {
          backgroundColor: this._radioGroup.fill || '',
          borderColor: this._radioGroup.fill || '',
          boxShadow: this._radioGroup.fill ? `-1px 0 0 0 ${this._radioGroup.fill}` : '', // 注意es6模板语法
          color: this._radioGroup.textColor || ''
        };
      },
      // 父组件的size
      size() {
        return this._radioGroup.size;
      },
      // 如果本身是true,听本身的，如果父级是true,听父级的。
      isDisabled() {
        return this.disabled || this._radioGroup.disabled;
      }
    }
  };
</script>
