<template>
  <div class="" :style="`--stripeColor:${strip}`">
    <Table ref="table" stripe  :size="'small'" :columns="columnsData" :data="tableData" :row-class-name="ttt"></Table>
  </div>
</template>

<script>
  import FuncCompMixin from '@/mixin/FuncCompMixin'

  export default {
    name: 'ITableComp',
    mixins: [FuncCompMixin],
    attr: {
      configDataTemp: {
        headerBackgroundColor: '',
        headerColor: '',
        tableConfig: {
          columns: [
            {
              type: 'selection',
              width: 60,
              align: 'center'
            },
            {
              title: 'Name',
              key: 'name'
            },
            {
              title: 'Sex',
              key: 'sex'
            }
          ]
        },
        customJsCode: '',
      }
    },
    data() {
      return {
        columnsData: [

        ],
        tableData: [
          {
            name: 'John Brown',
            age: 18,
            address: 'New York No. 1 Lake Park',
            date: '2016-10-03'
          },
          {
            name: 'Jim Green',
            age: 24,
            address: 'London No. 1 Lake Park',
            date: '2016-10-01'
          },
          {
            name: 'Joe Black',
            age: 30,
            address: 'Sydney No. 1 Lake Park',
            date: '2016-10-02'
          },
          {
            name: 'Jon Snow',
            age: 26,
            address: 'Ottawa No. 2 Lake Park',
            date: '2016-10-04'
          }
        ],
      }
    },
    created(){ },
    mounted() {
      let _this = this;
      this.setHeaderStyle()
      console.log(this.component);
  
      let newTableConfig = Object.assign({}, this.component.compConfigData.tableConfig);

      newTableConfig.columns.map(data=>{
        if (data.type) {
          this.columnsData.push({
            type: data.type,
            width: 60
          })
        }else {
          this.columnsData.push({
            title: data.title,
            key: data.key
          })
        }

      });

      eval(this.component.compConfigData.customJsCode)

    },
    methods: {
      ttt (row, index) {
        //console.log(row);
      },

      setHeaderStyle () {
        console.log(this.component.compConfigData.headerBackgroundColor);
        
        $(this.$refs.table.$refs.header).find('th').css({
          'background-color': this.component.compConfigData.headerBackgroundColor,
          'color': this.component.compConfigData.headerColor
        })
    
      },
    },
    computed: {
     strip(){
       return this.component.compConfigData.headerBackgroundColor
     }
    },
    watch: {
      'component.compConfigData.headerBackgroundColor': {
        handler: 'setHeaderStyle',
        deep: true
      },
      'component.compConfigData.headerColor': {
        handler: 'setHeaderStyle',
        deep: true
      },
    }
  }
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
 >>> .ivu-table-stripe .ivu-table-body tr:nth-child(2n) td, .ivu-table-stripe .ivu-table-fixed-body tr:nth-child(2n) td{
   background-color: var(--stripeColor);
  }
</style>
