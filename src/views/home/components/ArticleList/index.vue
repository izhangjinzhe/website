<template>
  <div class="card p-2">
    <div v-loading="loading" style="min-height: 300px" class="position-relative">
    <div class="d-flex align-items-center mb-2">
      <div class="me-auto">
        <router-link tag="span" to="/home/ask" class="finger btn btn-sm">提问</router-link>
        <span class="mx-2 opacity-25">|</span>
        <router-link tag="span" to="/home/share" class="finger btn btn-sm">分享</router-link>
        <span class="mx-2 opacity-25">|</span>
        <router-link tag="span" to="/home/advice" class="finger btn btn-sm">建议</router-link>
      </div>
      <div class="fs-7 d-flex justify-content-end">
        <span class="finger" :class="{'link-primary': sort === 0 }" @click="changeSort(0)">按最新</span>
        <span class="mx-2">|</span>
        <span class="finger" :class="{'link-primary': sort === 1}" @click="changeSort(1)">按热议</span>
      </div>
    </div>
      <article-item class="mb-2" v-for="item in list" :key="item.id" :data="item" type="list"></article-item>
    </div>
    <div class="mt-2 text-center">
      <button type="button" class="btn btn-outline-primary btn-sm me-2" @click="prev" :disabled="page === 1">上一页</button>
      <span class="me-2 fs-7">当前页: {{page}}</span>
      <button type="button" class="btn btn-outline-primary btn-sm" @click="next" :disabled="nextDisable">下一页</button>
    </div>
  </div>
</template>

<script>
import ArticleItem from '@/views/home/components/ArticleList/Item'
export default {
  name: 'ArticleList',
  components: { ArticleItem },
  data () {
    return {
      loading: true,
      nextDisable: false,
      sort: 0,
      page: 1,
      list: []
    }
  },
  props: {
    type: {
      type: String,
      default: 'index'
    }
  },
  created () {
    this.getList()
  },
  watch: {
    type () {
      this.getList()
    }
  },
  methods: {
    changeSort (value) {
      if (this.sort === value) return
      this.sort = value
      this.getList()
    },
    prev () {
      this.page--
      this.getList()
    },
    next () {
      this.page++
      this.getList()
    },
    async getList () {
      this.loading = true
      const { data } = await this.$fetch.get('/public/article_list', { type: this.type, page: this.page, sort: this.sort })
      this.list = data.data.list
      this.nextDisable = this.list.length < 10
      this.loading = false
    }
  }
}
</script>
