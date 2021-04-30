<template>
  <div id="app">
    <!-- <router-view/> -->
    <sl-button>的撒按时</sl-button>
    <sl-button :native-type="'reset'"></sl-button>

    <sl-checkbox-group v-model="checkGroup"
                       @change="handleChange"
                       :data-source="checkData"
                       class="check-group">
      <sl-checkbox-group-item @checked="handleCheck"
                              v-for="(checkData, i) in checkData"
                              :checked="i == 3 || i== 4"
                              :key="i"
                              :item="checkData">
        <template v-slot:default="props">
          <div :class="{'is-check': props.checked}"
               class="btn">
            {{props.scoped.label}}
          </div>
        </template>
      </sl-checkbox-group-item>
    </sl-checkbox-group>

    <sl-radio-group v-model="check"
                    @change="radioChange"
                    :data-source="checkData"
                    class="check-group">
      <sl-radio-group-item @checked="handleCheck"
                           v-for="(checkData, i) in checkData"
                           :key="i"
                           :item="checkData">
        <template v-slot:default="props">
          <div :class="{'is-check': props.checked}"
               class="btn">
            {{props.scoped.label}}
          </div>
        </template>
      </sl-radio-group-item>
    </sl-radio-group>

    <!-- <sl-tabs :tabList="tabList"
             :tabIndex="tabIndex"
             @changeTab="changeTab">
      <keep-alive>
        <component :is="currentContent">
        </component>
      </keep-alive>
    </sl-tabs> -->
    <!-- <sl-message>
      0.0
    </sl-message> -->
    <button type="submit"
            @click="handleClick">点击</button>
    <sl-tabs v-model="tabActive"
             @tab-click="tabClick">
      <template v-slot:content>
        <div>
          <sl-tabs-item label="w "
                        value="1">0.0</sl-tabs-item>
          <sl-tabs-item label="q"
                        value="2">0002</sl-tabs-item>
          <sl-tabs-item label="r"
                        value="3">fdsfds</sl-tabs-item>
          <sl-tabs-item label="y"
                        value="4">rrrr</sl-tabs-item>
          <sl-tabs-item label="n"
                        value="5">qqq</sl-tabs-item>
        </div>
      </template>
    </sl-tabs>

    <sl-radio-group v-model="checkRadio"
                    @change="handleRadio">
      <sl-radio-group-item :item="data"
                           v-for="data in checkData">
        <span style="margin: 0 15px 0 0">
          {{data.label}}
        </span>
      </sl-radio-group-item>
    </sl-radio-group>
  </div>
</template>

<script>
import Vue from "vue"
import SlButton from "../packages/button/src/button"
import SLCheckBoxGroup from "../packages/checkbox/src/checkbox-group"
import SLCheckBoxGroupItem from "../packages/checkbox/src/checkbox-group-item"
import SLRadioGroup from "../packages/radio/src/radio-group"
import SLRadioGroupItem from "../packages/radio/src/radio-group-item"
import SLTabs from "../packages/tabs/src/tabs"
import SLTabsItem from "../packages/tabs/src/tabs-item"
import SLMessage from "../packages/message/src/message.vue"

let MessageConstructor = Vue.extend(SLMessage);

export default {
  components: {
    SlButton,
    "sl-checkbox-group": SLCheckBoxGroup,
    "sl-checkbox-group-item": SLCheckBoxGroupItem,
    "sl-radio-group": SLRadioGroup,
    "sl-radio-group-item": SLRadioGroupItem,
    "sl-tabs": SLTabs,
    "sl-tabs-item": SLTabsItem,
    "sl-message": SLMessage
  },
  data () {
    return {
      checkedNames: [],
      checkGroup: ["1"],
      check: "",
      checkRadio: "3",
      checkData: [{
        label: "广州",
        value: "0"
      }, {
        label: "深圳",
        value: "1"
      }, {
        label: "上海",
        value: "2"
      }, {
        label: "北京",
        value: "3"
      }, {
        label: "成都",
        value: "4"
      }],
      tabIndex: 0,
      currentContent: 'one',
      tabList: [
        {
          index: 0,
          name: '选项一',
          component: 'one'
        },
        {
          index: 1,
          name: '选项二',
          component: 'two'
        }
      ],
      instance: null,
      instances: [],
      seed: 0,
      tabActive: ""
    }
  },
  mounted () {

  },
  methods: {
    handleChange (group) {
      console.log(group);

    },
    radioChange () {

    },
    handleCheck () {

    },
    handleClick () {
      console.log("点击");
      this.$message("测试1")
    },
    // 切换选项卡
    changeTab: function (tab) {
      this.tabIndex = tab.index
      this.currentContent = tab.component
    },
    tabClick (val) {
      console.log(val);

    },
    handleRadio (val) {
      console.log(val);
      console.log(this.checkRadio);

    }
  }
}
</script>

<style lang="scss">
.check-group {
  .sl-checkbox-group-item {
    .btn {
      width: 100px;
      height: 30px;
      line-height: 30px;
      margin: 14px;
      border: 1px solid #ccc;
      text-align: center;
    }
    .is-check {
      border-color: #6a9be4;
    }
  }
}
</style>
