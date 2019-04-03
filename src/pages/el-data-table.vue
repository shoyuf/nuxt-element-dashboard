<template>
  <div>
    <el-data-table
      :url="url"
      :columns="columns"
      :searchForm="searchForm"
      :onDelete="onDelete"
      :hasView="false"
      :hasEdit="false"
      :extraButtons="extraButtons"
      ref="elDataTable"
    >
    </el-data-table>
  </div>
</template>
<script>
import ElDataTable from 'el-data-table'
import {formatDate} from '@/const/filter'

export default {
  name: 'el-data-table-page',
  components: {ElDataTable},
  mounted() {},
  data() {
    const h = this.$createElement
    const _this = this
    return {
      url: '/edt-mock/components',
      columns: [
        {type: 'selection'},
        {prop: 'name', label: '组件名称'},
        {prop: 'type', label: '分类'},
        {prop: 'version', label: '版本'},
        {prop: 'language', label: '开发语言'},
        {
          prop: 'last_update_time',
          label: '最后更新时间',
          formatter(row) {
            return formatDate(row.last_update_time, 'YYYY-MM-DD')
          }
        },
        {
          prop: 'status',
          label: '状态',
          formatter(row) {
            if (row.status) return h('span', {style: {color: '#0c0'}}, '上架')
            return h('span', {style: {color: '#999'}}, '上架')
          }
        }
      ],
      searchForm: [
        {
          $id: 'name',
          $type: 'input',
          label: '组件名称',
          $el: {
            placeholder: '请输入'
          }
        },
        {
          $id: 'type',
          $type: 'select',
          label: '分类',
          $options: [
            {
              label: 'type1',
              value: 'type1'
            },
            {
              label: 'type2',
              value: 'type2'
            }
          ],
          $el: {
            placeholder: '请选择'
          }
        },
        {
          $id: 'status',
          $type: 'select',
          label: '状态',
          $options: [
            {
              label: '上架',
              value: 'status1'
            },
            {
              label: '下架',
              value: 'status2'
            }
          ],
          $el: {
            placeholder: '请选择'
          }
        }
      ],
      extraButtons: [
        {
          type: 'primary',
          size: 'mini',
          text: '查看',
          atClick: () => Promise.resolve()
        },
        {
          text: '编辑',
          atClick: () => this.$axios.put('/edt-mock/components/edit')
        },
        {
          text: '上架',
          atClick: () => this.$axios.put('/edt-mock/components/up'),
          show: row => !row.status
        },
        {
          text: '下架',
          atClick: () => this.$axios.put('/edt-mock/components/down'),
          show: row => row.status
        }
      ]
    }
  },
  methods: {
    onDelete(selected) {
      return this.$axios.delete('/edt-mock/components/delete', {
        data: {
          rows: selected.id || selected.map(v => v.id)
        }
      })
    }
  }
}
</script>
<style scoped></style>
