<template>
  <div class :style="`--stripeColor:${stripColor}`">
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
        tableConfig: {
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
          ]
        },
        customJsCode: "",
        strip: ""
      },
      PnDesigner.buildFuncCompDatasourceField({
        // 如果组件需要配置数据源，则需要调用此函数构建数据源字段对象
        ds_resultObjTemplate: [
          {
            field: "value",
            remark: "值"
          }
        ],
        ds_resultObj: [
          {
            value: 60
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
      ],
      stripColor: ""
    };
  },
  created() {},
  mounted() {
    let _this = this;
    this.setHeaderStyle();
    this.setStripColor();
    this.setColumn()
  
    eval(this.component.compConfigData.customJsCode);
  },
  methods: {
    ttt(row, index) {
      //console.log(row);
    },

    // 重置头部样式
    setHeaderStyle() {
      console.log(this.component.compConfigData.headerBackgroundColor);

      $(this.$refs.table.$refs.header)
        .find("th")
        .css({
          "background-color": this.component.compConfigData
            .headerBackgroundColor,
          color: this.component.compConfigData.headerColor
        });
    },

    // 重置表格表头
    setColumn() {
      this.columnsData = []
      let newTableConfig = Object.assign(
        {},
        this.component.compConfigData.tableConfig
      );

      newTableConfig.columns.map(data => {
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
    }
  },
  computed: {  },
  watch: {
    "component.compConfigData.headerBackgroundColor": {
      handler: "setHeaderStyle",
      deep: true
    },
    "component.compConfigData.tableConfig.columns": {
      handler: "setColumn",
      deep: true
    },
    "component.compConfigData.strip": {
      handler: "setStripColor",
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
>>> .ivu-table-stripe .ivu-table-body tr:nth-child(2n) td,
.ivu-table-stripe .ivu-table-fixed-body tr:nth-child(2n) td {
  background-color: var(--stripeColor);
}
>>> .ivu-table th {
  background-color: var(--headerBackgroundColor);
}
</style>
