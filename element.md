# 一、Table 表格

## 1.相同单元列合并

①.html部分

```html
<el-table
          :data="tableData"
          height="100%"
          border
          style="width: 100%"
          :header-cell-style="headerCellstyle"
          :cell-style="cellStyle"
          :span-method="objectSpanMethod"
        >
          <el-table-column prop="xuhao" label="序号" width="100">
          </el-table-column>
          <el-table-column prop="dataid" label="数据来源"> </el-table-column>
          <el-table-column prop="equipId" label="设备编号" width="200">
          </el-table-column>
          <el-table-column prop="equipType" label="设备类型" width="200">
          </el-table-column>
          <el-table-column prop="camp" label="阵营" width="150">
          </el-table-column>
          <el-table-column label="状态" width="180">
            <template slot-scope="scope">
              <div style="color: #67c23a" v-if="scope.row.state == 1">
                <i class="el-icon-success"></i> 在线
              </div>
              <div style="color: #f56c6c" v-else>
                <i class="el-icon-error"></i> 离线
              </div>
            </template>
          </el-table-column>
          <el-table-column prop="frequency" label="发射频率" width="180">
          </el-table-column>
          <el-table-column prop="dataAll" label="数据量(条)" width="180">
          </el-table-column>
          <el-table-column label="管理" width="200">
            <template slot-scope="scope">
              <el-switch
                style="display: block"
                v-model="scope.row.istration"
                active-color="#13ce66"
                inactive-color="#ff4949"
                active-text="上线"
                inactive-text="离线"
              >
              </el-switch>
            </template>
          </el-table-column>
          <el-table-column label="操作" align="center" min-width="100">
            <template slot-scope="scope">
              <el-button
                type="primary"
                plain
                @click="checkDetail(scope.row, $event)"
                >查看详情</el-button
              >
            </template>
          </el-table-column>
        </el-table>
```

②.json数据

```json
      tableData: [
        {
          xuhao: 1,
          dataid: "5308",
          equipId: 101010101,
          equipType: "侦打一体机器狗",
          camp: "红军",
          state: "1",
          frequency: "9 次/s",
          dataAll: "600,509",
          istration: true,
        },
        {
          xuhao: 2,
          dataid: "西南分院",
          equipId: 101010102,
          equipType: "单兵",
          camp: "红军",
          state: "0",
          frequency: "10 次/s",
          dataAll: "710,000",
          istration: false,
        },
        {
          xuhao: 3,
          dataid: "西南分院",
          equipId: 101010109,
          equipType: "单兵",
          camp: "红军",
          state: "1",
          frequency: "9 次/s",
          dataAll: "660,535",
          istration: true,
        }
      ],
      headerCellstyle: {
        background: "#eef1f6",
        color: "#606266",
        fontSize: "18px",
      },
      cellStyle: { "font-size": "16px" },
      currentPage4: 4,
      // 合并所用到的数据
      id_array: [],
      id_pos: 0,
```

③.js部分

```js
  mounted() {
    this.id_init();
  },    
// methods
    id_init() {
      this.id_array = [];
      this.id_pos = 0;
      for (let i = 0; i < this.tableData.length; i++) {
        // 如果当 i == 0 说明数据是第一行, 需要重新赋值
        if (i == 0) {
          // this.id_array.push(1) 说明这一行数据被显示出来
          this.id_array.push(1);
          // this.id_pos = 0 重置当前的计数器
          this.id_pos = 0;
        }
        // 说明不是从第一行开始遍历的
        else {
          // 判断当前的指定数据是否和之前的指定数据值相同
          if (this.tableData[i].dataid == this.tableData[i - 1].dataid) {
            // 如果相同就需要将 this.id_array 的数据自加
            this.id_array[this.id_pos] += 1;
            // 同时需要将 this.id_array push一个0 表示下一行不用显示
            this.id_array.push(0);
          }
          // 说明 当前的数据和上一行的指定数据不同
          else {
            // this.id_array.push(1) 说明当前一行的数据需要显示
            this.id_array.push(1);
            // 重新给计数器赋值
            this.id_pos = i;
          }
        }
      }
    },
    objectSpanMethod({ row, column, rowIndex, columnIndex }) {
      // 用于给第一列的table判断是否合并
      if (columnIndex === 1) {
        // this.id_array[rowIndex] 取出当前存放行的合并状态
        const _row = this.id_array[rowIndex];
        // 判断当前的 列是否需要显示
        const _col = _row > 0 ? 1 : 0;
        return {
          rowspan: _row,
          colspan: _col,
        };
      }
    },
```

# 二、Button 按钮

## 1.取消按钮颜色点击不回弹

```html
<el-button
                type="primary"
                plain
                @click="checkDetail(scope.row, $event)"
                >查看详情</el-button
              >
```

```js
checkDetail(val, evt) {
      let target = evt.target;
      if (target.nodeName == "SPAN" || target.nodeName == "I") {
        target = evt.target.parentNode;
      }
      target.blur();
    },
```

