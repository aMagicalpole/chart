<template>
  <div class :style="style">
    <Table
      ref="table"
      stripe
      :size="'small'"
      :columns="columnsData"
      :data="tableData"
      :row-class-name="ttt"
    ></Table>
  </div>
</template>

<script>
import FuncCompMixin from "@/mixin/FuncCompMixin";
import PnDesigner from "@/utils/PnDesigner";

export default {
  name: "ITableComp",
  mixins: [FuncCompMixin],
  attr: {
    configDataTemp: Object.assign(
      {
        headerBackgroundColor: "",
        headerColor: "",
        columns: [
          {
            type: "selection",
            width: 60,
            align: "center"
          },
          {
            title: "Name",
            key: "name"
          },
          {
            title: "Sex",
            key: "sex"
          }
        ],
        bodyBackgroundColor: "",
        bodyColor: "",
        customJsCode: "",
        strip: "",
        stripeHoverBackground: ""
      },
      PnDesigner.buildFuncCompDatasourceField({
        // 如果组件需要配置数据源，则需要调用此函数构建数据源字段对象
        ds_resultObjTemplate: [
          {
            field: "name",
            remark: "名称"
          },
          {
            field: "age",
            remark: "年龄"
          },
          {
            field: "address",
            remark: "地点"
          },
          {
            field: "date",
            remark: "时间"
          }
        ],
        ds_resultObj: [
          {
            name: "John Brown",
            age: 18,
            address: "New York No. 1 Lake Park",
            date: "2016-10-03"
          },
          {
            name: "Jim Green",
            age: 24,
            address: "London No. 1 Lake Park",
            date: "2016-10-01"
          },
          {
            name: "Joe Black",
            age: 30,
            address: "Sydney No. 1 Lake Park",
            date: "2016-10-02"
          },
          {
            name: "Jon Snow",
            age: 26,
            address: "Ottawa No. 2 Lake Park",
            date: "2016-10-04"
          }
        ]
      }),
      PnDesigner.buildFuncCompGlobalField()
    )
  },
  data() {
    return {
      columnsData: [],
      tableData: [
        {
          name: "John Brown",
          age: 18,
          address: "New York No. 1 Lake Park",
          date: "2016-10-03"
        }
      ],
      stripColor: "",
      headerBackgroundColor: "",
      headerColor: "",
      bodyBackgroundColor: "",
      bodyColor: "",
      stripeHoverBackground: ""
    };
  },
  created() {},
  mounted() {
    let _this = this;
    this.init();
    eval(this.component.compConfigData.customJsCode);
  },
  methods: {
    ttt(row, index) {
      //console.log(row);
    },

    // 重设表格数据
    setData() {
      this.tableData = [];
      this.component.compConfigData.ds_resultObj.map(item => {
        this.tableData.push(item);
        return item;
      });
    },

    // 重置头部背景色
    setHeaderStyle() {
      this.headerBackgroundColor = this.component.compConfigData.headerBackgroundColor;
    },

    // 重置头部字体颜色
    setHeaderColor() {
      this.headerColor = this.component.compConfigData.headerColor;
    },

    // 设置表格背景色
    setBodyStyle() {
      this.bodyBackgroundColor = this.component.compConfigData.bodyBackgroundColor;
    },

    // 设置表格字体颜色
    setBodyColor() {
      this.bodyColor = this.component.compConfigData.bodyColor;
    },

    // 重置表格表头
    setColumn() {
      this.columnsData = [];

      this.component.compConfigData.columns.map(data => {
        if (data.type) {
          this.columnsData.push({
            type: data.type,
            width: 60
          });
        } else {
          this.columnsData.push({
            title: data.title,
            key: data.key
          });
        }
      });
    },

    // 重置斑马纹样式
    setStripColor() {
      this.stripColor = this.component.compConfigData.strip;
    },

    // 重置斑马纹hover背景色
    setStripBackgroundColor() {
      this.stripeHoverBackground = this.component.compConfigData.stripeHoverBackground;
    },

    init() {
      this.setHeaderStyle();
      this.setHeaderColor();
      this.setStripColor();
      this.setStripBackgroundColor();
      this.setColumn();
      this.setData();
    }
  },
  computed: {
    style() {
      return `
      --stripeColor:${this.stripColor};
      --stripeHoverBackground:${this.stripeHoverBackground};
      --headerBackgroundColor:${this.headerBackgroundColor};
      --headerColor:${this.headerColor};
      --bodyBackgroundColor:${this.bodyBackgroundColor};
      --bodyColor:${this.bodyColor};
      `;
    }
  },
  watch: {
    "component.compConfigData.headerBackgroundColor": {
      handler: "setHeaderStyle"
    },
    "component.compConfigData.headerColor": {
      handler: "setHeaderColor"
    },
    "component.compConfigData.bodyBackgroundColor": {
      handler: "setBodyStyle"
    },
    "component.compConfigData.bodyColor": {
      handler: "setBodyColor"
    },
    "component.compConfigData.columns": {
      handler: "setColumn",
      deep: true
    },
    "component.compConfigData.strip": {
      handler: "setStripColor"
    },
    "component.compConfigData.stripeHoverBackground": {
      handler: "setStripBackgroundColor"
    },
    "component.compConfigData.ds_resultObj": {
      handler: "setData",
      deep: true
    }
  }
};
</script>

<style scoped>
/*>>> .ivu-table td {
    background-color: #182328;
    color: #fff;
  }
  >>> .ivu-table-header th{

    font-weight: bold;
    border: none;
  }*/
>>> .ivu-table td {
  background-color: var(--bodyBackgroundColor);
  color: var(--bodyColor);
}

>>> .ivu-table-stripe .ivu-table-body tr.ivu-table-row-hover td,
.ivu-table-stripe .ivu-table-fixed-body tr.ivu-table-row-hover td {
  background-color: var(--stripeHoverBackground);
}

>>> .ivu-table-stripe .ivu-table-body tr:nth-child(2n) td,
.ivu-table-stripe .ivu-table-fixed-body tr:nth-child(2n) td {
  background-color: var(--stripeColor);
}
>>> .ivu-table th {
  background-color: var(--headerBackgroundColor);
  color: var(--headerColor);
}
</style>
