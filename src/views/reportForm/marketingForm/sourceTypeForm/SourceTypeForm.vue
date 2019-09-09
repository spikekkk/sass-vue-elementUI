<template>
  <div>
    <FormSearch
      ref="formSearch"
      v-bind="formSearchProps"
      :loading="loading"
    />
    <Media :query="{minWidth: 980}">
      <CommonTable
        ref="sourceType"
        :columns="columns"
        :table-height="'calc(100vh - 150px)'"
        :pagination="listQuery"
        :options="options"
      />
    </Media>
    <Media :query="{maxWidth: 980}">
      <CommonTable
        ref="sourceType"
        :columns="columns"
        :table-height="'calc(100vh - 200px)'"
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
import { getSourceType } from '@/api/reportForm/sourceType'
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
        apiService: getSourceType, // 表格的数据请求接口
        params
      },
      columns: [
        {
          label: '来源类别',
          prop: 'channel',
          isShowTooltip: true
        }, {
          label: 'Leads数',
          prop: 'leads',
          isShowTooltip: true
        }, {
          label: '到访数',
          prop: 'visit',
          isShowTooltip: true
        }, {
          label: '到访比',
          prop: 'visitRatio',
          isShowTooltip: true
        }, {
          label: '签单数',
          prop: 'purNum',
          isShowTooltip: true
        }, {
          label: '到访签单比',
          prop: 'visitRatioPur',
          width: '120px',
          isShowTooltip: true
        }, {
          label: '签单金额',
          prop: 'orderMoney',
          isShowTooltip: true
        }, {
          label: '均单金额',
          prop: 'avgPurMoney',
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
      this.$refs.sourceType.getList(payload)
    },

    /** 导出 */
    exportFile() {
      const params = {
        ...this.searchValue
      }
      exportFile(`${window.REPORT_URL}/erp/tmk/export/sourceType`, params)
    }
  }
}
</script>

<style lang="scss" scoped>
.form-table {
  margin-top: 20px;
}
</style>
