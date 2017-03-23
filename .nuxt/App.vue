<template>
  <div id="__nuxt">
    <nuxt-loading ref="loading"></nuxt-loading>
    <component v-if="layout" :is="layout"></component>
  </div>
</template>

<script>
import NuxtLoading from './components/nuxt-loading.vue'

let layouts = {

  "_default": process.BROWSER_BUILD ? () => System.import('D:\\Workspace\\commerce\\marka-studio.ru\\tmp_adminlte_nuxtjs\\layouts\\default.vue') : require('D:\\Workspace\\commerce\\marka-studio.ru\\tmp_adminlte_nuxtjs\\layouts\\default.vue')

}

export default {
  head: {"meta":[{"charset":"utf-8"},{"name":"viewport","content":"width=device-width, initial-scale=1"}],"link":[{"rel":"icon","href":"http://placehold.it/50"},{"rel":"stylesheet","href":"https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css"},{"rel":"stylesheet","href":"https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.5.0/css/font-awesome.min.css"},{"rel":"stylesheet","href":"https://cdnjs.cloudflare.com/ajax/libs/admin-lte/2.3.11/css/AdminLTE.min.css"},{"rel":"stylesheet","href":"https://cdnjs.cloudflare.com/ajax/libs/admin-lte/2.3.11/css/skins/skin-blue.css"}],"script":[{"type":"text/javascript","src":"https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.3/jquery.min.js","body":true},{"type":"text/javascript","src":"https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/js/bootstrap.min.js","body":true},{"type":"text/javascript","src":"https://cdnjs.cloudflare.com/ajax/libs/admin-lte/2.3.11/js/app.min.js","body":true}]},
  data: () => ({
    layout: null,
    layoutName: ''
  }),
  
  mounted () {
    this.$loading = this.$refs.loading
    this.$nuxt.$loading = this.$loading
  },
  
  methods: {
    setLayout (layout) {
      if (!layout || !layouts['_' + layout]) layout = 'default'
      this.layoutName = layout
      let _layout = '_' + layout
      if (typeof layouts[_layout] === 'function') {
        return this.loadLayout(_layout)
      }
      this.layout = layouts[_layout]
      return Promise.resolve(this.layout)
    },
    loadLayout (_layout) {
      return layouts[_layout]()
      .then((Component) => {
        layouts[_layout] = Component
        this.layout = layouts[_layout]
        return this.layout
      })
      .catch((e) => {
        if (this.$nuxt) {
          return this.$nuxt.error({ statusCode: 500, message: e.message })
        }
        console.error(e)
      })
    }
  },
  components: {
    NuxtLoading
  }
}
</script>


