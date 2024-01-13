<template>
  <div>
    <div class="row">
      <edit-parameter @childClick="submit"> </edit-parameter>
  </div>
  <hr>
  <div></div>
  <div class="row">
        <div class="col-12">
        <card :title="table1.title">
          <div slot="raw-content" class="table-responsive">
            <paper-table :data="table1.data" :columns="table1.columns">
            </paper-table>
            <hr/>
          <div class="cus_eq"> Average Daily Price:   {{ avg_day_p }}</div>
          </div>
          
        </card>
      </div>
      </div>
  </div>
  
</template>
<script>
import EditParameter from "./UserProfile/EditParameter.vue";
import axios from 'axios';
import { PaperTable } from "@/components";
const tableColumns = [
    'Level_Seat_Number',
    "Price",
    "Price_Level",
  ];
const tableData = [];

export default {
  components: {
    EditParameter,
    PaperTable,
    
  },
  data() {
      return {
        avg_day_p: null,
        dataFromChild: {},
        table1: {
          title: "艙位價格預估",
          columns: [...tableColumns],
          data: [...tableData],
        },
        postData: {
          date: "2023-12-25",
          demand: 180,
          AbsentRate: 0.09,
          flight_id: 1,

        },
      };
     
    },

  methods: {
    fetchData(){

      this.postData.date = this.dataFromChild.date;
      this.postData.demand = this.dataFromChild.demand;
      this.postData.AbsentRate = this.dataFromChild.not_show_rate;

      axios.post('http://34.125.243.130:5000/set_parameter', this.postData)
        .then(res => {
          // handle the response data
          // this.avg_day_p = res.data.average_daily_price;
          console.log(this.avg_day_p);
          console.log(res.data);
          tableData.length = 0;
          for(let i = 0; i < res.data.cabin_info.length; i++) {
                const inputData = {};
                inputData.level_seat_number = res.data.cabin_info[i].level_seat_num;
                inputData.price = res.data.cabin_info[i].price;
                inputData.price_level = res.data.cabin_info[i].price_level;

                tableData.push(inputData);
              
           }
           this.table1.data = [...tableData];
           this.avg_day_p = res.data.avaerge_daily_price;
          // console.log(res.data);
          console.log(tableData);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },
  submit(data){
    this.dataFromChild = data;
    this.fetchData();
  },
    
  },
};
  
</script>
<style></style>
