<template>
  <div class="about">
    <wisdom-select placeholder="请选择" :optionsData="options" @selectChange="changeValue" />

<!--    <el-select v-model="value" placeholder="请选择" @change="changeSelect">-->
<!--      <el-option-->
<!--              v-for="item in options"-->
<!--              :key="item.value"-->
<!--              :label="item.text"-->
<!--              :value="item.value">-->
<!--      </el-option>-->
<!--    </el-select>-->
  </div>
</template>
<script>
  import WisdomSelect from "../components/wisdomSelect";

  export default {
    components: {WisdomSelect},
    data() {
      return {
        selectValue: '',
        options: [{
          value: '选项1',
          text: '黄金糕'
        }, {
          value: '选项2',
          text: '双皮奶'
        }, {
          value: '选项3',
          text: '蚵仔煎'
        }, {
          value: '选项4',
          text: '龙须面'
        }, {
          value: '选项5',
          text: '北京烤鸭'
        }],
        value: ''
      }
    },
    methods: {
      changeValue(e) {
        console.log(e)
      },
      // changeSelect(e){
      //   console.log(e, this.value);
      // }
    }
  }
</script>
<style scoped lang="scss">

</style>