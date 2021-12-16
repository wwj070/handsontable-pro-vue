<template>
  <div id="hot-preview" ref="testHota">
    <div class="top">
      <el-button type="primary" @click="add">新增</el-button>
      <el-button type="danger" @click="del">删除</el-button>
      <el-button @click="clear">清空</el-button>
    </div>
    <HotTable ref="hottable" :data='tabledata' :root="root" :settings="hotSettings"></HotTable>
  </div>
</template>

<script>
import { HotTable } from '@handsontable-pro/vue'
import '../../node_modules/handsontable-pro/dist/handsontable.full.css'
// import Handsontable from 'handsontable-pro'

export default {
  data: function() {
    // let that = this
    return {
      root: 'test-hot',
      tabledata: [        //数据可以是二维数组，也可以是数组对象
        [false, '20080101', 10, 11, 12, 13, ['查看 修改 提交EOA 删除']],
        [false, '20090101', 20, 11, 14, 13, true],
        [false, '20010101', 30, 15, 12, 13, true],
        [false, '20010101', 42, 213, 21, 312, true],
        [false, '20010201', 52, 213, 21, 312, true],
        [false, '20010301', 62, 213, 21, 312, true],
        [false, '20010401', 72, 213, 21, 312, true],
        [false, '20010501', 82, 213, 21, 312, true],
        [false, '20010601', 92, 213, 21, 312, true]
      ],
      hotSettings: {
        // data: [['sample', 'data']],
        // colHeaders: true,
        // rowHeaders: ['ID', 'Name', 'Address'], // 每行的标题，如果不够，则用大写字母补充
        startRows: 10,//行列范围
        startCols: 6,
        // minRows: 5,  //最小行列 影响删除行
        minCols: 5,
        // maxRows: 20,  //最大行列
        maxCols: 20,
        rowHeaders: true,//行表头，可以使布尔值（行序号），可以使字符串（左侧行表头相同显示内容，可以解析html），也可以是数组（左侧行表头单独显示内容）。
        colHeaders: ['选择', '题目', 'A选项', 'B选项', 'C选项', 'D选项', '操作',],//自定义列表头or 布尔值
        // copyPaste: true,
        copyPaste: {
          columnsLimit: 25,
          rowsLimit: 50,
          pasteMode: 'shift_down',
          uiContainer: document.body,
        },
        minSpareCols: 2, //列留白
        // minSpareRows: 2,//行留白
        // currentRowClassName: 'currentRow', //为选中行添加类名，可以更改样式
        // currentColClassName: 'currentCol',//为选中列添加类名
        autoWrapRow: true, //自动换行
        // hiddenRows: { // 隐藏行
        //   rows: [0, 1],
        //   indicators: false, // 官网说必须为true 才会隐藏，然而实际上并不是
        //   copyPasteEnabled: false // 为false 时跳过复制
        // },
        contextMenu: {
          items: {
            "row_above": {
              name: '上方插入一行'
            },
            "row_below": {
              name: '下方插入一行'
            },
            "col_left": {
              name: '左方插入列'
            },
            "col_right": {
              name: '右方插入列'
            },
            "hsep1": "---------", //提供分隔线
            "remove_row": {
              name: '删除行',
            },
            "remove_col": {
              name: '删除列',
            },
            "make_read_only": {
              name: '只读',
            },
            "borders": {
              name: '表格线'
            },
            "copy": {
              name: '复制'
            },
            // "paste": {
            //   name: '粘贴',
            //   callback: function() {
            //     let me = that.$refs.testHota
            //     console.log(me, that);
            //     var plugin = me.getPlugin('copyPaste');
            //     me.listen();
            //     plugin.paste(11);
            //     console.log("paste回调")
            //   }
            // },
            "cut": {
              name: '剪切'
            },
            "commentsAddEdit": {
              name: '添加备注',
            },
            "commentsRemove": {
              name: '取消备注',
            },
            "freeze_column": {
              name: '固定列',
            },
            "unfreeze_column": {
              name: '取消列固定',
            },
            "undo": { name: "撤销" },
            "redo": { name: "恢复" },
            "hsep2": "---------",
          }
        },
        manualColumnFreeze: true, //手动固定列  ?
        manualColumnMove: true, //手动移动列
        manualRowMove: true,   //手动移动行
        manualColumnResize: true,//手工更改列距
        manualRowResize: true,//手动更改行距
        comments: true, //添加注释  ?
        cell: [  //???
          { row: 1, col: 1, comment: { value: 'this is test' } },
        ],
        customBorders: [],//添加边框
        columnSorting: true,//排序
        stretchH: 'all',//根据宽度横向扩展，last:只扩展最后一列，none：默认不扩展
        fillHandle: true, //选中拖拽复制 possible values: true, false, "horizontal", "vertical"
        fixedColumnsLeft: 2,//固定左边列数
        // fixedRowsTop: 2,//固定上边列数
        mergeCells: [   //合并
          // {row: 1, col: 1, rowspan: 3, colspan: 3},  //指定合并，从（1,1）开始行3列3合并成一格
          // {row: 3, col: 4, rowspan: 2, colspan: 2}
        ],
        columns: [     //添加每一列的数据类型和一些配置
          { type: 'checkbox' },  //多选框
          {
            type: 'date',   //时间格式
            dateFormat: 'YYYY-MM-DD',
            correctFormat: true,
            defaultDate: '19000101'
          },
          {
            type: 'dropdown', //下拉选择
            source: ['BMW', 'Chrysler', 'Nissan', 'Suzuki', 'Toyota', 'Volvo'],
            strict: false   //是否严格匹配
          },
          { type: 'numeric' },  //数值
          {
            type: 'numeric',
            readOnly: true  //设置只读
          },
          {
            type: 'numeric',
            format: '$ 0,0.00'
          },  //指定的数据格式
          {},

        ],
      }
    };
  },
  mounted() {
    this.$refs.hottable.hotInstance.addHook('afterOnCellMouseDown', this.cellClick)
  },
  methods: {
    cellClick(a, b) {
      this.tabledata[b.row][0] = !this.tabledata[b.row][0]
      this.$refs.hottable.hotInstance.loadData(this.tabledata)
    },
    add() {
      this.$refs.hottable.hotInstance.alter('insert_row')
    },
    del() {
      let arr = JSON.parse(JSON.stringify(this.tabledata))
      let delcount = 0
      for (let i = 0; i < arr.length; i++) {
        if (arr[i][0]) {
          this.$refs.hottable.hotInstance.alter('remove_row', i - (delcount++))
        }
        if (!this.tabledata.length) {
          this.$refs.hottable.hotInstance.alter('insert_row')
        }
      }
    },
    clear() {
      let me = this
      this.tabledata = [[]]
      this.$nextTick(() => {
        me.tabledata = [[false]]
      })
    }
  },
  components: {
    HotTable
  }
}
</script>

<style scoped>
.top {
  margin: 20px;
}
</style>