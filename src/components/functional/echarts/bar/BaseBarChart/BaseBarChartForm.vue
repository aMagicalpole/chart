<template>
  <div class>
    <Tabs size="small">
      <TabPane label="主要配置" :style="{paddingTop: '10px'}">
        <Collapse value="1">
          <Panel name="1">
            网格
            <div slot="content">
              <Form :label-width="100">
                <FormItem label="左边距">
                  <Input size="small" v-model="left" />
                </FormItem>
                <FormItem label="上边距">
                  <Input size="small" v-model="top" />
                </FormItem>
                <FormItem label="右边距">
                  <Input size="small" v-model="right" />
                </FormItem>
                <FormItem label="下边距">
                  <Input size="small" v-model="bottom" />
                </FormItem>
              </Form>
            </div>
          </Panel>
          <Panel name="2">
            标题配置
            <div slot="content">
              <Form :label-width="100">
                <FormItem label="是否显示">
                  <i-switch v-model="titleShow">
                    <span slot="open"></span>
                    <span slot="close"></span>
                  </i-switch>
                </FormItem>
                <FormItem label="文本">
                  <Input size="small" v-model="titleText" />
                </FormItem>
              </Form>
            </div>
          </Panel>

          <Panel name="3">
            x轴配置
            <div slot="content">
              <Form :label-width="100">
                <FormItem label="显示">
                  <i-switch v-model="xAxisShow">
                    <span slot="open"></span>
                    <span slot="close"></span>
                  </i-switch>
                </FormItem>
                <FormItem label="文本颜色">
                  <ColorPicker size="small" v-model="xAxisAxisLabelColor" alpha recommend />
                </FormItem>
                <FormItem label="辅助线">
                  <i-switch v-model="xAxisAxisLineShow">
                    <span slot="open"></span>
                    <span slot="close"></span>
                  </i-switch>
                </FormItem>
                <FormItem label="辅助线颜色">
                  <ColorPicker size="small" v-model="xAxisAxisLineLineStyleColor" alpha recommend />
                </FormItem>
              </Form>
            </div>
          </Panel>
          <Panel name="4">
            y轴配置
            <div slot="content">
              <Form :label-width="100">
                <FormItem label="显示">
                  <i-switch v-model="yAxisShow">
                    <span slot="open"></span>
                    <span slot="close"></span>
                  </i-switch>
                </FormItem>
              </Form>
            </div>
          </Panel>
          <Panel name="5">
            数据系列
            <div slot="content">
              <Form :label-width="100">
                <FormItem label="系列颜色">
                  <ColorPicker size="small" v-model="color" alpha recommend />
                </FormItem>
              </Form>
            </div>
          </Panel>
        </Collapse>
      </TabPane>
      <TabPane label="数据源" :style="{paddingTop: '10px'}">
        <DatasourceState targetComp="BaseBarChart"></DatasourceState>
      </TabPane>
    </Tabs>
  </div>
</template>

<script>
import FuncCompFormMixin from "@/mixin/FuncCompFormMixin";

import { createHelpers } from "vuex-map-fields";

const { mapFields } = createHelpers({
  getterType: "designer/getLayoutItem",
  mutationType: "designer/updateLayoutItem"
});

export default {
  name: "BaseBarChartForm",
  mixins: [FuncCompFormMixin],
  data() {
    return {};
  },
  mounted() {},
  methods: {},
  computed: {
    ...mapFields({
      color: "component.compConfigData.chartOption.color",

      /* title配置 */
      // title: 'component.compConfigData.chartOption.title',
      titleShow: "component.compConfigData.chartOption.title.show",
      titleText: "component.compConfigData.chartOption.title.text",

      /* grid配置 */
      // grid: 'component.compConfigData.chartOption.grid',
      left: "component.compConfigData.chartOption.grid.left",
      top: "component.compConfigData.chartOption.grid.top",
      right: "component.compConfigData.chartOption.grid.right",
      bottom: "component.compConfigData.chartOption.grid.bottom",

      /* x轴配置 */
      // xAxis: 'component.compConfigData.chartOption.xAxis',
      xAxisShow: "component.compConfigData.chartOption.xAxis.show",
      xAxisAxisLabelColor:
        "component.compConfigData.chartOption.xAxis.axisLabel.color",
      xAxisAxisLineShow:
        "component.compConfigData.chartOption.xAxis.axisLine.show",
      xAxisAxisLineLineStyleColor:
        "component.compConfigData.chartOption.xAxis.axisLine.lineStyle.color",

      /* y轴配置 */
      // yAxis: 'component.compConfigData.chartOption.yAxis',
      yAxisShow: "component.compConfigData.chartOption.yAxis.show"
    })
  }
};
</script>

<style scoped>
.ivu-form-item {
  margin-bottom: 0px;
}
</style>
