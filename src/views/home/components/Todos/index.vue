/**
 * 待办事项
 */
<template>
  <el-card
    v-loading="loading"
    shadow="never"
    element-loading-text="拼命加载中"
  >
    <div class="todo-title">
      <h3>待办事项</h3>
      <i
        :style="{fontSize: '16px'}"
        class="iconfont icon_gb_jgsz icon-common"
        @click="$emit('openTodoSet')"
      />
    </div>
    <ul class="todo-list">
      <li
        v-for="(item, index) in todoList"
        :key="'todo'+index"
        class="todo-item"
      >
        <span :class="renderClass(item.type)">{{ renderType(item.type) }}</span>
        <a @click="() => {goPage(item.link)}">{{ item.content }}</a>
      </li>
      <li
        v-if="todoList.length === 0"
        style="text-align:center;paddinf-top:10px"
      >
        <img
          src="https://img.ishanshan.com/gimg/n/20190716/32abc0bf9318778546d66059c80ecd4d"
          alt=""
        >
        <div style="color:#999;font-size:12px;padding:5px 0 10px 0">暂无待办事项</div>
      </li>
    </ul>
  </el-card>
</template>

<script>
import { getSysConfByKey } from '@/api/home/home'
import { mapMutations } from 'vuex'
export default {

  props: {
    loading: {
      type: Boolean,
      required: true
    },
    todoList: {
      type: Array,
      required: true
    }
  },
  data() {
    return {

    }
  },

  mounted() {
    this.$emit('didMount', 'todo')
    this.queryContractExpire()
  },

  methods: {
    ...mapMutations('home', [
      'UPDATE_STATE'
    ]),
    queryContractExpire() {
      getSysConfByKey({ confKey: 'purExpireRemind' }).then(res => {
        if (res.data.errorCode === 0) {
          const results = res.data.data.results
          let key = 0
          if (results && results.length >= 0) {
            key = results[0].key
          }
          this.UPDATE_STATE({ contractExpire: key })
        }
      })
    },
    renderType(type) {
      if (type === 'game_num' || type === 'market_num') {
        return '招生'
      } else if (type === 'touch_goal') {
        return '跟进'
      } else if (type === 'birthday') {
        return '生日'
      } else if (type === 'period_less') {
        return '课时'
      } else if (type === 'contract_expire') {
        return '合同'
      } else if (type === 'contract_audit' || type === 'receivables_audit' || type === 'refund_audit' || type === 'class_give' ||
        type === 'leave_audit' || type === 'transfer_audit') {
        return '审核'
      } else if (type === 'sign_num') {
        return '签到'
      } else if (type === 'class_today') {
        return '上课'
      }
    },

    renderClass(type) {
      if (type === 'touch_goal' || type === 'contract_audit' || type === 'receivables_audit' || type === 'refund_audit' || type === 'class_give' ||
        type === 'leave_audit' || type === 'transfer_audit') {
        return 'green-type'
      } else if (type === 'birthday' || type === 'period_less') {
        return 'orange-type'
      } else {
        return ''
      }
    },

    /** 跳转页面 */
    goPage(link) {
      const linkList = link.split('?')
      if (linkList.length === 3) {
        console.log(linkList)
        // console.log(linkList[1] === 'contractOrder' || linkList[1] === 'stuAccount')
        // if (linkList[1] === 'contractOrder' || linkList[1] === 'stuAccount') {
        //   this.$router.push({ name: linkList[0], params: { activeTab: linkList[1], action: linkList[2], queryType: 'lessen' }})
        // } else {
        this.$router.push({ name: linkList[0], params: { activeTab: linkList[1], action: linkList[2] }})
        // }
      } else if (linkList.length === 2) {
        console.log(linkList)
        this.$router.push({ name: linkList[0], params: { activeTab: linkList[1] }})
      } else if (linkList.length === 1) {
        this.$router.push({ name: linkList[0] })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/styles/mixin.scss";

.todo-title {
  @include flex;
  @include flexCenter(space-between);

  h3 {
    font-size: 16px;
    line-height: 24px;
    color: #333;
  }
  .icon-common {
    cursor: pointer;
    color: #bbb;
    font-size: 14px;
    line-height: 24px;
    margin-left: 6px;
    @include transition(color 0.4s);
    &:hover {
      color: #56c0f5;
    }
  }
}

.todo-list {
  margin-top: 20px;
}
.todo-item {
  margin-top: 14px;

  &:first-child {
    margin: 0;
  }

  span {
    border: 1px solid #46b6ee;
    border-radius: 12px;
    color: #46b6ee;
    padding: 2px 10px;
    font-size: 12px;
    margin-right: 10px;
  }
  .green-type {
    border-color: #88c70a;
    color: #88c70a;
  }
  .orange-type {
    border-color: #f5a623;
    color: #f5a623;
  }
  a {
    color: #666;
    &:hover {
      color: #46b6ee;
    }
  }
}
</style>
