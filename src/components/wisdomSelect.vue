<template>
  <div class="wisdom-select">
    <div class="select-header" @click="isExpand=!isExpand">
      <input type="text" autocomplete="off" readonly :placeholder="placeholder" :value="placeholderValue"
             @keydown.down.prevent="selectChildWidthArrowDown" @keydown.up.prevent="selectChildWidthArrowUp"
             @keydown.enter="selectChild">
      <i :class="isExpand?'el-icon-arrow-up':'el-icon-arrow-down'"></i>
    </div>
    <div class="select-body" v-if="isExpand">
      <transition name="el-fade-in-linear" mode="out-in">
        <div class="typeahead-filter" v-show="typeaheadData">
          <transition-group tag="ul" name="el-fade-in-linear">
            <li v-for="(item,index) in typeaheadData " :key="index" :class="{ 'active':item.active}"
                @mouseenter="setActiveClass(index)" @mouseleave="setActiveClass(index)" @click="selectChild(index)">
              <a href="javascript:;">
                {{item.text}}
              </a>
            </li>
          </transition-group>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'wisdomSelect',
    data() {
      return {
        placeholderValue: '',
        // 是否展开
        isExpand: false,
        // 当前默认选中的index,
        currentIndex: -1,
      }
    },
    computed: {
      typeaheadData() {
        if (!this.value) {
          return this.mapData
        } else {
        }
      }
    },
    props: {
      // value: {
      //   required: true
      // },
      placeholder: {
        type: String,
        default: '请选择'
      },
      mapData: {
        type: Array,
        default: function () {
          return []
        }
      }
    },
    methods: {
      // 重置opction
      resetDefaultStatus() {
        this.currentIndex = -1;
        this.typeaheadData.forEach(item => {
          this.$set(item, 'active', false);
        })
      },
      // opction选项 鼠标事件
      setActiveClass(index) {
        this.mapData.forEach((item, innerIndex) => {
          if (index === innerIndex) {
            // 添加激活的状态
            this.$set(item, 'active', true);
            // 重置上下键的索引
            this.currentIndex = index;
          } else {
            this.$set(item, 'active', false)
          }
        })
      },
      // opction选项 下键盘事件
      selectChildWidthArrowDown() {
        if (this.currentIndex < this.typeaheadData.length) {
          this.currentIndex++;
          this.typeaheadData.forEach((item, index) => {
            this.currentIndex === index ? this.$set(item, 'active', true) : this.$set(item, 'active', false);
          })
        }
      },
      // opction选项 上键盘事件
      selectChildWidthArrowUp() {
        if (this.currentIndex > 0) {
          this.currentIndex--;
          this.typeaheadData.forEach((item, index) => {
            this.currentIndex === index ? this.$set(item, 'active', true) : this.$set(item, 'active', false);
          })
        }
      },
      // 鼠标点击选择
      selectChild(index) {
        this.mapData.forEach((item, innerIndex) => {
          if (index === innerIndex || item.active) {
            this.placeholderValue = item.text;
            this.isExpand = false;
            // this.$emit('update:value', item.value)
            this.$emit('selectChange', item.value);
          }
          this.$set(item, 'active', false);
        })
        this.resetDefaultStatus();
      },
    }
  }
</script>

<style scoped lang="scss">
  .el-fade-in-linear-enter-active,
  .el-fade-in-linear-leave-active,
  .fade-in-linear-enter-active,
  .fade-in-linear-leave-active {
    transition: opacity .2s linear;
  }

  .el-fade-in-enter,
  .el-fade-in-leave-active,
  .el-fade-in-linear-enter,
  .el-fade-in-linear-leave,
  .el-fade-in-linear-leave-active,
  .fade-in-linear-enter,
  .fade-in-linear-leave,
  .fade-in-linear-leave-active {
    opacity: 0;
  }


  .wisdom-select {
    width: 240px;

    a {
      color: #333;
      text-decoration: none;
      padding: 5px;
    }

    ul {
      list-style: none;
      padding: 6px 0;
      margin: 0;
      overflow: visible;

      li {
        display: block;
        width: 100%;
        font-size: 14px;
        padding: 8px 10px;
        position: relative;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        color: #48576a;
        height: 36px;
        line-height: 1.5;
        box-sizing: border-box;
        cursor: pointer;

        &.active {
          background-color: #20a0ff;

          a {
            color: #fff;
          }
        }
      }
    }

    .select-header {
      position: relative;
      border-radius: 4px;
      border: 1px solid #bfcbd9;
      outline: 0;
      padding: 0 8px;

      > input {
        border: none;
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        width: 100%;
        outline: 0;
        box-sizing: border-box;
        color: #1f2d3d;
        font-size: inherit;
        height: 36px;
        line-height: 1;
      }

      > i {
        transition: all .3s linear;
        display: inline-block;
        position: absolute;
        right: 3%;
        top: 50%;
        transform: translateY(-50%);
      }
    }

    .select-body {
      z-index: 1000;
      border-radius: 2px;
      background-color: #fff;
      box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);
      box-sizing: border-box;
      margin: 5px 0;
      padding: 8px;
    }
  }
</style>
