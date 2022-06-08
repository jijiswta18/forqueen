<template>
    <div class="report-page pa-2">
      <div class="box-btn">
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
          </v-icon>ลงนามถวายพระพร
        </v-btn>

      <v-btn
        color="blue-grey"
        class="btn-export"
      >
        <vue-excel-xlsx
            class="btn btn-default"
            :data="data"
            :columns="columns"
            :file-name="'forqueen'"
            :file-type="'xlsx'"
            :sheet-name="'forqueen'"
            >
            Export Excel
        </vue-excel-xlsx>
      </v-btn>
      </div>

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
                    :footer-props="{'items-per-page-options':[10, 30, 50, 100, -1]}"
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
    components: {
  },
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
        columns : [
            {
                label: "ลำดับ",
                field: "number",
            },
            {
                label: "ชื่อ-สกุล",
                field: "name",
            },
            {
                label: "Browser",
                field: "browser",
            },
            {
                label: "Device",
                field: "device",
            },
            {
                label: "วันที่ลงนาม",
                field: "regis_date",
            }
        ],
      }
    },
    created(){
      this.getDetail()
    },
   
    methods:{
        getDetail: async function(){
            const response = await this.$axios.get('/api/export_ffuagvylst')  
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
          this.$router.push('/')
        },

    }
  }
</script>
<style scoped>
  .report-page{
      padding: 12px;
  }
  .backhome{
    background-color: #f57c00!important;
    border-color: #f57c00!important;
    color: white!important;
  }
  .btn-export{
    border: 1px solid #f57c00!important;
     color: #f57c00!important;
  }
  .box-btn{
    text-align: right;
    margin-bottom: 1rem;
  }
</style>