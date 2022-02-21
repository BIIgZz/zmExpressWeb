<template >


  <a-card :bordered="false">
    <div>
      <div style="margin-left:10px" id="app" ref="spreadsheet"></div>
    </div>
  </a-card>
</template>
<script>

import jexcel from 'jspreadsheet'
import 'jspreadsheet/dist/jspreadsheet.css'
var data = [
  ['民警考核绩效考核报表', '', '', '', '', '', '', '', '', '', '', '', ''],
  ['单位及部门', '', '考核内容及分值( 100)', '', '', '', '', '', '', '', '', '', '备注'],
  [
    '部门',
    '姓名',
    '共性考核(18%)/默认100分(满分100)',
    '',
    '',
    '岗位绩效考核(39%)/默认80分(满分100分)',
    '',
    '',
    '专项绩效考核(3%)/满分(5)',
    '领导考核(40%)/(满分100分)',
    '考核成绩',
    '考核等次',
    ''
  ],
  ['', '', '普通扣分', '否决指标', '合计', '加分', '扣分', '合计', '', '', '', '', '']
]
var options = {
  data: data,
  allowToolbar: true,
  isEditable: true,
  colWidths: [100, 100, 130, 130, 130, 130, 130, 130, 200, 230, 130, 130, 200],
  columns: [
    { type: 'text', title: '民警考核绩效考核报表', width: '120px' },
    { type: 'text', title: '姓名', width: '150px', source: ['Alfa Romeo', 'Audi', 'Bmw'] },
    { type: 'text', title: '普通扣分', width: '150px', },
    { type: 'text', title: '否决指标', width: '120px' },
    { type: 'text', title: '合计', width: '80px' },
    { type: 'numeric', title: '加分', width: '150px', mask: '$ #.##,00', decimal: ','},
    { type: 'text', title: '扣分', width: '120px' },
    { type: 'text', title: '合计', width: '120px' }
  ],

  //   nestedHeaders: [
  //     [
  //       {
  //         title: '民警考核绩效考核报表',
  //         colspan: '7'
  //       }
  //     ],
  //     [
  //       {
  //         title: 'Location',
  //         colspan: '5'
  //       },
  //       {
  //         title: ' Other Information',
  //         colspan: '2'
  //       }
  //     ]
  //   ]
  mergeCells: {
    A1: [13, 1], //合并单元格，第一个数字是合并列，第二个数字是合并行
    A2: [2, 1],
    C2: [10, 1],
    M2: [1, 3],
    A3: [1, 2],
    B3: [1, 2],
    C3: [3, 1],
    F3: [3, 1],
    I3: [1, 2],
    J3: [1, 2],
    K3: [1, 2],
    L3: [1, 2]
  },
  minDimensions: [10, 40]
}

export default {
  name: 'App',
  mounted: function() {
    let spreadsheet = jexcel(this.$refs.spreadsheet, options)
    Object.assign(this, { spreadsheet })
  }
}

</script>
<style >
</style>