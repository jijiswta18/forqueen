<template>
    <div class="report-page pa-2">
     <v-btn
        class="ma-2 backhome"
        color="orange darken-2"
        @click="backHome"
      >
        <v-icon
          dark
          left
        >
          mdi-arrow-left
        </v-icon>Back
      </v-btn>
        <div id="app" data-app>
            <v-card>
                <v-card-title>
                    <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search"
                    single-line
                    hide-details
                    ></v-text-field>
                </v-card-title>
                <v-data-table
                    :headers="headers"
                    :items="desserts"
                    :items-per-page="12"
                    :footer-props="{'items-per-page-options':[12, 30, 50, 100, -1]}"
                    :search="search"
                    class="elevation-1"
                ></v-data-table>
            </v-card>
        </div>
    </div> 
</template>
<script>
  import moment from 'moment'
  export default {
    data () {
      return {
        loader: null,
        loading: false,
        search: '',
        headers: [
          {
            text: 'ลำดับ',
            align: 'start',
            sortable: false,
            value: 'number',
          },
          { text: 'ชื่อ-สกุล', value: 'name' },
          { text: 'Browser', value: 'browser' },
          { text: 'Device', value: 'device' },
          { text: 'วันที่ลงนาม', value: 'regis_date' },
        ],
        desserts: [],
      }
    },
    created(){
      this.getDetail()
    },
   
    methods:{
        getDetail: async function(){
            const response = await this.$axios.get('/api/export_forking')  
            response.data.data.forEach(item => {
                this.desserts.push({ 
                    'number'    : item.id,
                    'name'      : item.p_name + ' ' + item.p_lastname,
                    'browser'   : item.browser,
                    'device'    : item.device,
                    'regis_date': moment(item.regis_date).format('YYYY-MM-DD HH:mm:ss'),
                })
            })
        },
        backHome(){
          this.$router.go(-1)
        }
    }
  }
</script>
<style scoped>
    .report-page{
        padding: 12px;
    }
  .backhome{
      text-align: right;
      display: block;
      margin-bottom: 1rem;
      background-color: #f57c00!important;
      border-color: #f57c00!important;
      color: white;
  }
</style>