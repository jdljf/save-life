<template>
  <div :data-value="value"
       class="sl-checkbox-group">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    value: { default: [] },
    dataSource: { default: [] },
    disabled: { default: false },
    // min: {
    //   type: Number,
    //   default: -1
    // },
    max: {
      type: Number
    }
  },
  methods: {
    //获取父级的v-model
    getModel () {
      let vModel
      if (Array.isArray(this.value)) {
        vModel = this.value;
      }
      else {
        vModel = this.value.toString().split(",");
      }
      return vModel
    },
    //清除冗余数值
    filterModel (vModel) {
      var dataSource = this.dataSource;
      var canSetValue = [];
      var dataSourceValue = [];
      for (var i = 0; i < dataSource.length; i++) {
        dataSourceValue.push(dataSource[i].value.toString()); //需要转为字符串，兼容数字类型值
      }
      vModel.forEach(function (item, index) {
        var index = dataSourceValue.indexOf(item);
        if (index > -1) {
          canSetValue.push(item);
        }
      });
      return canSetValue;
    },
    // 初始化父级的v-model。如果子组件有选中状态但没在v-model中就加入进去
    mountedCheckedModel (item) {
      let vModel = this.getModel()
      let index = vModel.indexOf(item.value);

      if (index < 0) {
        vModel.push(item.value);
      }
    },
    // 触发点击 checkbox 事件
    onChange (event, item) {
      //每次触发都先获取父级的v-model，保证多个选项的数据同步，v-model初始值中可能存在冗余数据，后面需要用filterModel过滤掉冗余数据。
      var vModel = this.getModel(), flag = false, groupIndex = -1;

      if (vModel.length > this.max) {
        return
      }

      if (this.dataSource.length <= 1) {
        vModel.splice(0, vModel.length);
      }
      var $obj = event.target;
      var index = vModel.indexOf($obj.value);

      // 如果是已选状态就加入
      if ($obj.checked) {
        if (index < 0) {
          vModel.push($obj.value);
          flag = true
          groupIndex = vModel.length - 1
        }
      }
      else {
        if (index >= 0) {
          vModel.splice(index, 1); //数组中移除
          flag = false
          groupIndex = index
        }
      }
      vModel = this.filterModel(vModel);//清除冗余数值
      if (!Array.isArray(this.value)) {
        vModel = vModel.join(",");//如果v-model是字符串，同步时候也转为字符串。
      }
      // console.log(vModel)
      this.$emit("change", vModel, item, groupIndex, flag);//同步父级的v-model
    },
    isChecked (val) {
      var vModel = this.getModel();
      //务必toString()后再查找，否则会和数值型不兼容。
      if (vModel.indexOf(val.toString()) > -1) {
        return true;
      }
      else {
        return false;
      }
    }
  },
}
</script>
