<template>
  <div>

    
       <!-- 下拉式选单 -->
        <dropdown @command="handleCommand">
      <span class="el-dropdown-link">
        選擇航班編號<i class="el-icon-arrow-down el-icon--right"></i>
      </span>
        <dropdown-menu slot="dropdown"  >
            <dropdown-item command="MM620">MM620</dropdown-item>   
            </dropdown-menu>
        </dropdown>
        <input class="input" placeholder="航班編號" type="text" v-model="id" readonly>

        <dropdown @command="handleCommand2">
      <span class="el-dropdown-link">
        選擇時間維度<i class="el-icon-arrow-down el-icon--right"></i>
      </span>
        <dropdown-menu slot="dropdown"  >
            <dropdown-item command="s">季</dropdown-item>   
            <dropdown-item command="m">月</dropdown-item>
            </dropdown-menu>
        </dropdown>
        <input class="input" placeholder="時間維度" type="text" v-model="duration" readonly>

       
        <button @click="submit(duration)"  class="search_button">查詢</button>
     

    <!--Charts-->
    <div class="row">
      <!-- 分成左右兩半 -->

        <div class="col-6">
        <chart-card
          title="過去收益"
          :chart-data="pastRevChart.data"
          :chart-options="pastRevChart.options"
        >
          <!-- <span slot="footer">
            <i class="ti-check"></i> Data information certified
          </span> -->
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> 過去收益
          </div>
        </chart-card>
        </div>
        <div class="col-6">
        <chart-card
          title="售票率"
          :chart-data="salesRateChart.data"
          :chart-options="salesRateChart.options"
        >
          <!-- <span slot="footer">
            <i class="ti-check"></i> Data information certified
          </span> -->
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> 售票率
          </div>
        </chart-card>
        </div>
    </div>
      <div class="row">
        <div class="col-6">
          <chart-card
          title="艙位收益分析"
          :chart-data="classRankChart.data"
          chart-type="Pie"
          >
          
          <div slot="legend">
            <!-- 可以有五種顏色 -->
            <i class="fa fa-circle text-info"></i> A
            <i class="fa fa-circle text-danger"></i> B
            <i class="fa fa-circle text-warning"></i> C
            <i class="fa fa-circle text-primary"></i> D
            <i class="fa fa-circle text-success"></i> E
          </div>
          </chart-card>
        </div>
        <div class="col-6">
          <chart-card
          title="地區收益貢獻度分析"
          :chart-data="regionRankChart.data"
          chart-type="Pie"
          >
        
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> {{ region_rank.r1 }}
            <i class="fa fa-circle text-danger"></i> {{ region_rank.r2 }}
            <i class="fa fa-circle text-warning"></i> {{ region_rank.r3 }}
            <i class="fa fa-circle text-primary"></i> {{ region_rank.r4 }}
            <i class="fa fa-circle text-success"></i> {{ region_rank.r5 }}
            <i class="fa fa-circle text-muted"></i> 其他
          </div>
          </chart-card>
        </div>
      </div>
      

     

      
    
  </div>
</template>
<script>
import { StatsCard, ChartCard } from "@/components/index";
import { Dropdown, DropdownMenu, DropdownItem, DatePicker, Button } from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
// import Chartist from "chartist";
import { PaperTable } from "@/components";
import axios from 'axios';
const past_rev =[];
const survival_rate =[];

export default {
  components: {
    StatsCard,
    ChartCard,
    PaperTable,
    Dropdown,
    DropdownMenu,
    DropdownItem,
    DatePicker,
    Button,
  },
  /**
   * Chart data used to render stats, charts. Should be replaced with server data
   */
  data() {
    return {
      postData: {
        time: '',
      },

      region_rank: {
        r1:"",
        r2:"",
        r3:"",
        r4:"",
        r5:"",

      },
      //下拉式選單相關
      id: null,
      duration: "",
      season_labels: [
            "2023 Q1",
            "2023 Q2",
            "2023 Q3",
            "2023 Q4",
      ],
      month_labels: [
            "1",
            "2",
            "3",
            "4",
            "5",
            "6",
            "7",
            "8",
            "9",
            "10",
            "11",
            "12",
      ],
      
      
     
      pastRevChart: {
        data: {
          labels: [
            "2023 Q1",
            "2023 Q2",
            "2023 Q3",
            "2023 Q4",
          ],
          series: [
            [...past_rev],
            // [230, 293, 380, 480, 503, 553, 600, 664, 698, 710, 736, 795],
          ],
        },
        options: {
          seriesBarDistance: 10,
          axisX: {
            showGrid: false,
          },
          height: "245px",
        },
      },
      salesRateChart: {
        data: {
          labels: [
          "2023 Q1",
            "2023 Q2",
            "2023 Q3",
            "2023 Q4",
          ],
          series: [
            [...survival_rate],
            // [230, 293, 380, 480, 503, 553, 600, 664, 698, 710, 736, 795],
          ],
        },
        options: {
          seriesBarDistance: 10,
          axisX: {
            showGrid: false,
          },
          height: "245px",
        },
      },
      classRankChart: {
        data: {
          labels: ["56%", "29%", "6%", "6%", "3%"],
          series: [56, 29, 6, 6, 3],
        },
        options: {},
      },
      regionRankChart: {
        data: {
          labels: ["56%", "29%", "6%", "6%", "3%"],
          series: [56, 29, 6, 6, 3],
        },
        options: {},
      },
      
    };
  },
  methods: {
    handleCommand(command) {
            console.log("sjlfjasljflasjlfjawlef");
            this.id = String(command);
        },
    handleCommand2(command) {
            console.log("時間尺度");
            if(String(command) == "s"){
              this.duration = "季";
            }else{
              this.duration = "月";
            }
            
        },
    submit(duration){
      console.log("Clicked Submit with duration:", duration);

      // this.getPastRev(duration);
      // this.getSalesRate(duration);
      
    },

    // 取得過去收益
    getPastRev(duration){
      this.postData.time = duration;
      const past_rev = [];
        // 感覺是後端出問題
        axios.post('http://34.80.114.185:5000/past_revenue',this.postData)
        .then(res => {
          // handle the response data

          //時間維度所帶來的 x 長度 （季 = 4; 月 = 12）
          let time_len = res.data.past_rev.length;

          if(time_len == 4){
            this.pastRevChart.data.labels = this.season_labels;
          }else{
            this.pastRevChart.data.labels = this.month_labels;
          }

          for(let i = 0; i < time_len; i++) {
            past_rev.push(Number(res.data.past_rev[i])|| 0);
            
          }
          this.pastRevChart.data.series = [[...past_rev]];
          console.log(this.tableData);
         
          console.log(res.data);
          console.log(past_rev);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    // 取得售票率
    getSalesRate(duration){
      this.postData.time = duration;
      const sales_rate = [];
        // 感覺是後端出問題
        axios.post('http://34.80.114.185:5000/get_sales_rate',this.postData)
        .then(res => {
          // handle the response data

          //時間維度所帶來的 x 長度 （季 = 4; 月 = 12）
          let time_len = res.data.past_rev.length;

          if(time_len == 4){
            this.salesRateChart.data.labels = this.season_labels;
          }else{
            this.salesRateChart.data.labels = this.month_labels;
          }

          for(let i = 0; i < time_len; i++) {
            sales_rate.push(Number(res.data.sales_rate_list[i])|| 0);
            
          }
          this.salesRateChart.data.series = [[...sales_rate]];
          console.log(this.tableData);
         
          console.log(res.data);
          console.log(sales_rate);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    //
  },
  mounted(){
    // this.getPastRev("季");
    // this.getSalesRate("季");

    // this.fetchData();
  },
};
</script>
<style lang="scss" >
    .input {
        border-radius: 4px;
        border: 1px solid #DCDFE6;
        padding: 8px 4px;
        margin: 0px 8px 12px 4px;
        text-align: center;
    }
    .search_button {
        // padding: 0.3rem 18px;
        border-radius: 4px;
        border: 1px solid #DCDFE6;
        margin: 0px 8px;
        padding: 8px 16px;
    }
    .el-input__inner {
      border: 1px solid #DCDFE6; 
      height: 40px; 
      background-color: #FFF;
    }
  </style>
