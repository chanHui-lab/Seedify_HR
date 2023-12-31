<template lang="pug">
v-row.justify-center.mx-auto
    v-card.py-4.px-6.rounded-lg(outlined)
      v-card-title.px-0.pb-0
        p.mb-0 Employee Performance
        v-spacer
      .d-flex.align-center
        p.mb-1.mr-2.primary--text 1 June 2023 - 1 July 2023
        eva-icon(name="calendar" :fill="$vuetify.theme.themes.light.primary" width="18" height="18")
      hr.mt-4
        //- v-text-field(
        //-   v-model="search"
        //-   append-icon="mdi-magnify"
        //-   label="Search"
        //-   single-line
        //-   hide-details
        //- )

      //- Datatable
      v-data-table.mt-18(
        :headers="headers"
        :items="empolyees"
        :search="search"
        multi-sort
        @click:row="onRowClick"
      )

        template(v-slot:body.prepend)
          tr
            td.py-4
              v-text-field(v-model="name" type="text" label="Employee Name" hide-details="auto" dense outlined)
            td.py-4
              v-text-field(v-model="total_sales" type="number" label="More than" hide-details="auto" dense outlined)
            td.py-4
              v-text-field(v-model="net_sales" type="number" label="More than" hide-details="auto" dense outlined)
            td.py-4
              v-text-field(v-model="total_commission" type="number" label="More than" hide-details="auto" dense outlined)
            td.py-4
              v-text-field(v-model="sales_growth" type="number" label="More than" hide-details="auto" dense outlined)
            td.py-4
              v-text-field(v-model="customer_reviews" type="number" label="More than" hide-details="auto" dense outlined)

            td.py-4
              v-select.select-category(:items="statusList" label="Select a status" v-model="performance_status" hide-details="auto" multiple chips dense outlined)
                template(v-slot:selection="{ item, index }")
                  v-chip(:color="getColor(item)" outlined)
                    span {{ item }}

        template(v-slot:item.name="{ item }")
          .d-flex.align-center
            VueAvatar(:username="item.name" :size="24" :colors="avatarColor" :customStyles="{'color': 'white'}")
            span.ml-2.body-2 {{item.name}}

        template(v-slot:item.total_sales="{ item }")
          p.mb-0 {{ $formatCurrency(item.total_sales) }}

        template(v-slot:item.net_sales="{ item }")
          p.mb-0 {{ $formatCurrency(item.net_sales) }}

        template(v-slot:item.total_commission="{ item }")
          p.mb-0 {{ $formatCurrency(item.total_commission) }}

        template(v-slot:item.sales_growth="{ item }")
          p.mb-0.danger--text(v-if="item.sales_growth < 0") {{ parseFloat(item.sales_growth * 100).toFixed (2) }}%
          p.mb-0.green--text(v-else) +{{ parseFloat(item.sales_growth * 100).toFixed (2) }}%

        template(v-slot:item.customer_reviews="{ item }")
          .d-flex.align-center.justify-center
            p.mb-1.orange--text {{ item.customer_reviews }}/10
            eva-icon(name="star" :height="16" :fill="$vuetify.theme.themes.light.orange")

        template(v-slot:item.performance_status="{ item }")
          v-chip.my-1(
            :color="getColor(item.performance_status)"
            outlined
            pill
          )
            p.mb-0 {{ item.performance_status }}

</template>

<script>
export default {
  name: 'StartupTable',
  data () {
    return {
      search: '',
      name: '',
      customer_reviews: '',
      net_sales: '',
      total_sales: '',
      total_commission: '',
      sales_growth: '',
      performance_status: [],
      statusList: [
        'All',
        'Excellent',
        'Good',
        'Poor'
      ],
      avatarColor: [[200, 54, 45], [192, 35, 69], [203, 38, 52], [189, 69, 67], [207, 46, 40], [193, 59, 48], [198, 34, 65], [197, 66, 66], [205, 41, 69], [180, 66, 59]],
      colors: [
        { name: 'Excellent', color: '#3d9970' },
        { name: 'Good', color: '#4C5175' },
        { name: 'Poor', color: '#BB0000' }
      ],
      headers: [
        {
          text: 'Employee Name',
          align: 'start',
          value: 'name',
          filter: (f) => { return (f + '').toLowerCase().includes(this.name.toLowerCase()) }
        },
        {
          text: 'Total Sales',
          align: 'end',
          value: 'total_sales',
          filter: (value) => {
            if (!this.total_sales) { return true }
            return value > parseInt(this.total_sales)
          }
        },
        {
          text: 'Total Net Sales',
          align: 'end',
          value: 'net_sales',
          filter: (value) => {
            if (!this.net_sales) { return true }
            return value > parseInt(this.net_sales)
          }
        },
        {
          text: 'Total Commission',
          align: 'end',
          value: 'total_commission',
          filter: (value) => {
            if (!this.total_commission) { return true }
            return value > parseInt(this.total_commission)
          }
        },
        {
          text: 'Sales Growth',
          align: 'end',
          value: 'sales_growth',
          filter: (value) => {
            if (!this.sales_growth) { return true }
            return value > parseInt(this.sales_growth)
          }
        },
        {
          text: 'Customer Review',
          align: 'center',
          value: 'customer_reviews',
          filter: (value) => {
            if (!this.customer_reviews) { return true }
            return value > parseInt(this.customer_reviews)
          }
        },
        {
          text: 'Status',
          align: 'center',
          sortable: false,
          value: 'performance_status',
          filter: (f) => {
            if (f !== '') {
              if (this.performance_status.length === 0 || this.performance_status.includes('All')) {
                return true
              }
              const result = this.performance_status.filter(value => f.includes(value))
              if (result.length > 0) {
                return true
              } else {
                return false
              }
            } else {
              return false
            }
          }
        }
      ],
      empolyees: require('../../assets/data/employee.json')
    }
  },
  methods: {
    getColor (status) {
      const result = this.colors.find((c) => { return c.name === status })
      if (result) {
        return result.color
      } else {
        return this.$vuetify.theme.themes.primary
      }
    },
    onRowClick (item) {
      console.log(this.empolyees)
      this.$router.push('/employee')
    }
  }
}
</script>

<style lang="scss" scoped>
.width-95 {
  width: 95% !important;
}

:deep(thead)  {
  background-color: rgba(91, 95, 151, .1);
}

:deep(.select-category .v-chip .v-chip__content) {
  font-size: 12px !important;
}

:deep(.select-category .v-chip.v-size--default) {
  height: 20px;
}

</style>
