<template>
  <keep-alive>
    <!-- 使用fullPath作为key，避免在keep-alive中，路由相同传，路径传参不同，却都共用一个vnode缓存 -->
    <router-view :key="$route.fullPath" ref="page" />
  </keep-alive>
</template>

<script>
export default {
  name: 'FullPathKeepAlive',
  mounted () {
    // 监听消息
    this.$multiTab.instance.$on('remove-cache', (fullPath) => {
      // 清楚缓存
      const {
        cache, keys
      } = this.$refs.page.$vnode.parent.componentInstance
      if (cache[fullPath]) {
        delete cache[fullPath]
      }

      const index = keys.indexOf(fullPath)
      if (index > -1) {
        keys.splice(index, 1)
      }
    })
  }
}
</script>
