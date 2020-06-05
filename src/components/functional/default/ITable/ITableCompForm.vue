<template>
  <div class>
    <Tabs size="small">
      <!-- 基础配置 -->
      <TabPane label="基础配置" :style="{paddingTop: '10px'}">
        <Form :label-width="105">
          <FormItem label="表头背景色">
            <ColorPicker size="small" v-model="headerBackgroundColor" alpha recommend />
          </FormItem>
          <FormItem label="表头字体颜色">
            <ColorPicker size="small" v-model="headerColor" alpha recommend />
          </FormItem>
            <FormItem label="表格背景色">
            <ColorPicker size="small" v-model="bodyBackgroundColor" alpha recommend />
          </FormItem>
          <FormItem label="表格字体颜色">
            <ColorPicker size="small" v-model="bodyColor" alpha recommend />
          </FormItem>
          <!-- <FormItem label="显示斑马纹">
              <i-switch v-model="stripe">
                <span slot="open"></span>
                <span slot="close"></span>
              </i-switch>
          </FormItem>-->
          <FormItem label="斑马线背景色">
            <ColorPicker size="small" v-model="strip" alpha recommend />
          </FormItem>
          <FormItem label="鼠标放上背景色">
            <ColorPicker size="small" v-model="stripeHoverBackground" alpha recommend />
          </FormItem>
           <FormItem label>
            <Button
              size="small"
              type="primary"
              @click="customJsCodeModalVisible = !customJsCodeModalVisible"
            >自定义代码</Button>
          </FormItem>
          <FormItem label>
            <Button
              size="small"
              type="primary"
              @click="customTableColumnVisible = !customTableColumnVisible"
            >自定义表头</Button>
          </FormItem>
        </Form>

        <Modal
          v-model="customJsCodeModalVisible"
          draggable
          scrollable
          title="自定义代码编辑"
          width="650"
          :mask="true"
          :z-index="3"
        >
          <CodeEditor v-model="customJsCode"></CodeEditor>
          <div slot="footer">
            <Button type="default" @click="customJsCodeModalVisible = false">关闭</Button>
          </div>
        </Modal>

        <!-- 表头编辑 -->
        <Modal
          v-model="customTableColumnVisible"
          draggable
          scrollable
          title="表头编辑"
          width="650"
          :mask="true"
          :z-index="3"
        >
          <CodeEditor v-model="tableColumnData" mode="json" :on-blur="updateResultObj"></CodeEditor>
          <div slot="footer">
            <Button type="default" @click="customTableColumnVisible = false">关闭</Button>
          </div>
        </Modal>

      </TabPane>

      <!-- 数据源 -->
      <TabPane label="数据源" :style="{paddingTop: '10px'}">
        <DatasourceState targetComp="ITableComp"></DatasourceState>
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
  name: "ITableCompForm",
  mixins: [FuncCompFormMixin],
  data() {
    return {
      customJsCodeModalVisible: false,
      customTableColumnVisible: false,
      tableColumnData: ""
    };
  },
  mounted() {
    this.tableColumnData = JSON.stringify(this.columns, null, 2);
  },
  methods: {
    // 更新表头配置数据
    updateResultObj() {
        this.$store.commit('designer/updateLayoutItem', {
          path: 'component.compConfigData.columns',
          value: JSON.parse(this.tableColumnData)
        });
    }
  },
  computed: {
    ...mapFields({
      headerBackgroundColor: "component.compConfigData.headerBackgroundColor",
      headerColor: "component.compConfigData.headerColor",
      bodyBackgroundColor: "component.compConfigData.bodyBackgroundColor",
      bodyColor: "component.compConfigData.bodyColor",
      customJsCode: "component.compConfigData.customJsCode",
      columns: "component.compConfigData.columns",
      strip: "component.compConfigData.strip",
      stripeHoverBackground: "component.compConfigData.stripeHoverBackground"
    })
  }
};
</script>

<style scoped>
.ivu-form-item {
  margin-bottom: 0px;
}
</style>
