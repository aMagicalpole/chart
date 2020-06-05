<!--基本饼图-->

<template>
  <div class="w-h-full">
    <div v-resize="resizeHandle" style="width: 100%; height: 100%;">
      <div :id="'chart-'+component.id" class="w-h-full"></div>
    </div>
  </div>
</template>

<script>
import FuncCompMixin from "@/mixin/FuncCompMixin";
import ChartCompMixin from "@/mixin/ChartCompMixin";
import PnDesigner from "@/utils/PnDesigner";

const _this = {
  name: "BaseLineChart",
  mixins: [FuncCompMixin, ChartCompMixin],
  attr: {
    version: "1.1",
    configDataTemp: Object.assign(
      {
    
      },
      PnDesigner.buildFuncCompDatasourceField({
        ds_resultObjTemplate: [
          {
            field: "name",
            remark: "类目"
          },
          {
            field: "value",
            remark: "值"
          }
        ],
        ds_resultObj: [
          {
            name: "南宁",
            value: 10
          },
          {
            name: "柳州",
            value: 52
          },
          {
            name: "桂林",
            value: 200
          },
          {
            name: "崇左",
            value: 334
          },
          {
            name: "北海",
            value: 390
          },
          {
            name: "防城港",
            value: 330
          },
          {
            name: "玉林",
            value: 220
          }
        ]
      }),
      PnDesigner.buildFuncCompGlobalField()
    )
  },
  data() {
    return {};
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.initDatasource(
        () => {
          this.chart.setOption(this.chartOption);
        },
        () => {
          this.component.compConfigData.ds_resultObj = result.data;
          this.chart.setOption(this.chartOption);
        }
      );
    }
  },
  computed: {
    chartOption() {
      let option = {
        // tooltip: {
        //   trigger: "item",
        //   formatter: this.component.compConfigData.chartOption.tooltip.formatter
        // },
        xAxis: {
          type: "category",
          data: []
        },
        yAxis: {
          type: "value"
        },
        series: [
          {
            name: "",
            type: "line",
            data: [],
            smooth: true
          }
        ]
      };

      option.series[0].data = this.component.compConfigData.ds_resultObj.map(item=>item.value);
      option.xAxis.data = this.component.compConfigData.ds_resultObj.map(item=>item.name);
      console.log(option.series[0].data,option.xAxis.data);
      


      return option;
    }
  },
  watch: {}
};

export default _this;
</script>

<style scoped>
</style>
