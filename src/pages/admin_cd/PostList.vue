<template>
  <div>
    <el-tabs v-model="TabsValue" type="card">
      <el-tab-pane label="未审核" name="UnStared">
        <cd_post_list type="UnStared" user="CD"></cd_post_list>
      </el-tab-pane>
      <el-tab-pane label="已审核" name="Stared">
        <cd_post_list type="Stared" user="CD"></cd_post_list>
      </el-tab-pane>
      <el-tab-pane label="未通过" name="UnPassed">
        <cd_post_list type="UnPassed" user="CD"></cd_post_list>
      </el-tab-pane>
      <el-tab-pane label="所有" name="All">
        <cd_post_list type="All" user="CD"></cd_post_list>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
  import PostList from '../../components/public/PostList.vue'
  export default {
    components: {
      'cd_post_list': PostList
    },
    data () {
      return {
        TabsValue: 'UnStared'
      }
    },
    methods: {}
  }
</script>
<style>
</style>
