<template lang="pug">
.dashboard-page.pa-0.ma-0.fill-width
  upper-title.ma-0(:title="'Dashboard'" :titleClass="'dark-background'" :icon="'bell'" :rightIconColor="$vuetify.theme.themes.light.tertiary")
  v-row.ma-0.pt-8.fill-width
    v-col(cols="12")
      expenses-table-vue.pb-8.align-center.px-7.pt-12
</template>

<script>
import { mapGetters } from 'vuex'
import ExpensesTableVue from '~/components/employee/ExpensesTable.vue'
import UpperTitleVue from '~/components/UpperTitle.vue'

export default {
  name: 'ExpensesPage',
  components: {
    UpperTitleVue,
    ExpensesTableVue
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
