<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default{
  name: 'App'
}
</script>
<style>
.el-loading-mask.is-fullscreen {
    z-index: 14000!important
}
/* .el-loading-spinner .path{
  stroke: #fff!important
}
.el-loading-spinner .el-loading-text{
  color: #fff!important
} */
</style>
