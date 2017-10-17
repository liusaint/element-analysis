<template>
  <div id="app">  
    <router-view/>
     <ElAlert title="这是一个alert" show-icon description="this is description">1</ElAlert>
      <el-button type="primary" icon="edit" :disabled="true" @click="clickbtn">按钮</el-button>

      <label><input type="radio" name="" id="" value="1" v-model="a">测试label</label>
      <label><input type="radio" name="" id="" value="2" v-model="a">2222222222</label>
  </div>

</template>

<script>
export default {
  name: 'app',
  // data(){

  // },

  created(){
    // this.$alert(2);
  },
  methods:{
    clickbtn(){
      alert(1);
    }
  },
  watch: {
    a(value) {
        console.log(value);
      }
    },
    computed:{
      a: {
        get() {

          return 1;
        },
        set(value) {
          console.log('set');

        }
      },
    },
}
</script>

<style>
/*import 'element-ui.css';*/
@import './element-ui/lib/theme-default/index.css';
/*@import '../../node_modules/swiper/dist/css/swiper.min.css';*/
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
