<template>
  <div>
    <FormSearch
      ref="formSearch"
      v-bind="formSearchProps"
      :loading="loading"
    />
    <Media :query="{minWidth: 980}">
      <CommonTable
        ref="signType"
        :columns="columns"
        :table-height="'calc(100vh - 182px)'"
        :pagination="listQuery"
        :options="options"
      />
    </Media>
    <Media :query="{maxWidth: 980}">
      <CommonTable
        ref="signType"
        :columns="columns"
        :table-height="'calc(100vh - 230px)'"
        :pagination="listQuery"
        :options="options"
        class="form-table"
      />
    </Media>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import Media from 'vue-media'
import moment from 'moment'
import FormSearch from '@/components/ReportForm/FormSearch'
import CommonTable from '@/components/CommonTable/CommonTable'
import { getSignType } from '@/api/reportForm/signType'
import { exportFile } from '@/utils/exportFile'

export default {
  components: {
    Media,
    FormSearch,
    CommonTable
  },

  data() {
    const now = moment()
    const start = now.add('month', 0).format('YYYY-MM') + '-01'
    const end = moment(start).add('month', 1).add('days', -1).format('YYYY-MM-DD')
    const params = {
      startTime: start,
      endTime: end
    }

    return {
      formSearchProps: {
        mediaWidth: 980,
        defaultDate: [new Date(start), new Date(end)],
        searchHandle: this.makeForm,
        exportFile: this.exportFile
      },
      listQuery: {
        show: false // 是否显示
      },
      options: {
        apiService: getSignType, // 表格的数据请求接口
        params
      },
      columns: [
        {
          label: '签单类型',
          prop: 'typeName',
          isShowTooltip: true
          // formatter: (row, column, cellValue) => {
          //   const type = row.type + ''
          //   const text = type === '0' ? '新签' : type === '1' ? '新签-转介绍' : type === '2' ? '续费' : type === '3' ? '总计' : ''

          //   return `<div>${text}</div>`
          // }
        }, {
          label: '业绩金额',
          prop: 'orderMoney',
          isShowTooltip: true
        }, {
          label: '业绩占比',
          prop: 'perfPerce',
          isShowTooltip: true
        }, {
          label: '招生人数',
          prop: 'stuNum',
          isShowTooltip: true
        }, {
          label: '均单金额',
          prop: 'avgOrderMoney',
          isShowTooltip: true
        }, {
          label: '合同数',
          prop: 'purNum',
          isShowTooltip: true
        }, {
          label: '签单课时',
          prop: 'periodNum',
          isShowTooltip: true
        }, {
          label: '课时单价',
          prop: 'perPrice',
          isShowTooltip: true
        }
      ],
      searchValue: params
    }
  },

  computed: {
    ...mapState('commonTable', [
      'loading'
    ])
  },

  methods: {
    /** 生成报表 */
    makeForm(values) {
      const payload = {
        startTime: values.date[0],
        endTime: values.date[1]
      }
      this.searchValue = payload
      this.options.params = payload
      this.$refs.signType.getList(payload)
    },

    /** 导出 */
    exportFile() {
      const params = {
        ...this.searchValue
      }

      exportFile(`${window.REPORT_URL}/erp/tmk/export/signType`, params)
    }
  }
}
</script>

<style lang="scss" scoped>
.form-table {
  margin-top: 20px;
}
</style>
