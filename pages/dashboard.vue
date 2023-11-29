<template lang="pug">
.dashboard-page.pa-0.ma-0.fill-width
  upper-title.ma-0(:title="'Dashboard'" :titleClass="'dark-background'" :icon="'bell'" :rightIconColor="$vuetify.theme.themes.light.tertiary")
  v-row.ma-0.pt-8.fill-width
    v-col(cols="12")
      trend-list-vue.pt-12.align-center.px-4.pb-4
    v-col(cols="12")
      startup-table-vue.pb-8.align-center.px-7
</template>

<script>
import { mapGetters } from 'vuex'
import StartupTableVue from '~/components/dashboard/startupTable.vue'
import TrendListVue from '~/components/dashboard/trendList.vue'
import UpperTitleVue from '~/components/UpperTitle.vue'

export default {
  name: 'DashboardPage',
  components: {
    UpperTitleVue,
    TrendListVue,
    StartupTableVue
  },
  layout: 'default',
  data () {
    return {
      // search: null
    }
  },
  computed: {
    ...mapGetters({
    })
  },
  methods: {
  }
}
</script>

<style scoped>

</style>
