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
          title="預估收益"
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
            <i class="fa fa-circle" style="color: #F3BB45;"></i> B
            <i class="fa fa-circle" style="color: #EB5E28;"></i> C 
            <i class="fa fa-circle" style="color: #41B883;"></i> D 
            <i class="fa fa-circle" style="color: #7A9E9F;"></i> E 
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
            <i class="fa fa-circle" style="color: #F3BB45;"></i> {{ region_rank.r2 }}
            <i class="fa fa-circle" style="color: #EB5E28;"></i> {{ region_rank.r3 }}
            <i class="fa fa-circle" style="color: #41B883;"></i> {{ region_rank.r4 }}
            <i class="fa fa-circle" style="color: #7A9E9F;"></i> {{ region_rank.r5 }}
            <i class="fa fa-circle text-muted" style="color: rgba(104, 179, 200, 0.8);"></i> {{ region_rank.r6 }}
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
import Chartist from "chartist";
import { PaperTable } from "@/components";
import axios from 'axios';

const past_rev =[];
const survival_rate =[];
const labels= [
            "2023 Q1",
            "2023 Q2",
            "2023 Q3",
            "2023 Q4",
      ];
const season_labels= [
            "2023 Q1",
            "2023 Q2",
            "2023 Q3",
            "2023 Q4",
      ];
const  month_labels= [
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
      ];

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
        r1:"台北市",
        r2:"新北市",
        r3:"台南市",
        r4:"高雄市",
        r5:"台中市",
        r5:"其他",

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
          labels: [...labels],
          series: 
            // [...past_rev],
            [[33200, 29300, 38000, 48000]],
            // [230, 293, 380, 480, 503, 553, 600, 664, 698, 710, 736, 795],
          
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
          series: 
            // [...survival_rate],
            [[0.03, 0.019, 0.028, 0.05]],
          
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
          labels: ["56%", "6%", "29%", "6%", "3%"],
          series: [56, 6, 29, 6, 3],
        },
        options: {},
      },
      regionRankChart: {
        data: {
          labels: ["36%", "24%", "26%", "11%", "3%", "3%"],
          series: [36, 24, 26, 11, 3, 3],
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
      // this.pastRevChart.data.labels = [...month_labels];
      // this.pastRevChart.data.series[0] = [11200, 19300, 8000, 9000, 7200, 6300, 8000, 9200, 7200, 9300, 8000, 6500];
      // console.log(this.pastRevChart);
      this.getPastRev(duration);
      this.getSalesRate(duration);
      this.getClassRank(duration);
      this.getRegionRank(duration);
      
    },

    // 取得過去收益
    getPastRev(duration){
      this.postData.time = duration;
      const past_rev = [];
    
        // 感覺是後端出問題
        axios.post('http://34.125.243.130:5000/past_revenue',this.postData)
        .then(res => {
          // handle the response data

          //時間維度所帶來的 x 長度 （季 = 4; 月 = 12）
          const pastRev = res.data.past_rev;
          let time_len = pastRev.length;
          // console.log(time_len);
          
          if(time_len == 4){
            // this.$set(this.pastRevChart.data.labels, 'labels', season_labels);
            this.pastRevChart.data.labels = [...season_labels];
            // labels = season_labels;
            
          }else{
            // this.$set(this.pastRevChart.data.labels, 'labels', month_labels);
            this.pastRevChart.data.labels = [...month_labels];
          }

          // console.log(labels);
          for(let i = 0; i < time_len; i++) {
            past_rev.push(Number(res.data.past_rev[i])|| 0);
            
          }
          this.pastRevChart.data.series[0] = [...past_rev];
          // console.log("pastSeries");
          // console.log(this.pastRevChart.data.series);
         
          // console.log(res.data);
          // console.log(past_rev);
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
        axios.post('http://34.125.243.130:5000/get_sales_rate',this.postData)
        .then(res => {
          // handle the response data

          //時間維度所帶來的 x 長度 （季 = 4; 月 = 12）
          const salesRate = res.data.sales_rate_list;
          let time_len = salesRate.length;

          if(time_len == 4){
            this.salesRateChart.data.labels = [...season_labels];
          }else{
            this.salesRateChart.data.labels = [...month_labels];
          }

          for(let i = 0; i < time_len; i++) {
            sales_rate.push(Number(res.data.sales_rate_list[i])|| 0);
            
          }
          this.salesRateChart.data.series[0] = [...sales_rate];
          console.log(res.data);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    // 取得艙位排名
    getClassRank(duration){
      this.postData.time = duration;
      const class_rank = [];
      const class_rank_labels = [];
    
      // console.log("艙位排名");
        axios.post('http://34.125.243.130:5000/class_rank',this.postData)
        .then(res => {
          // handle the response data
          // console.log("進來了");
          // console.log(res);
          for(let i = 0; i < 5; i++) {
            class_rank.push(Number(res.data.rank[i].value)|| 0);
            class_rank_labels.push(res.data.rank[i].rate);
            
          }
          this.classRankChart.data.labels = [...class_rank_labels];
          this.classRankChart.data.series = [...class_rank];
          // console.log("class_rank");
          // console.log(this.classRankChart.data);
         
          
          
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },
     // 取得區域排名
     getRegionRank(duration){
      this.postData.time = duration;
      const region_rank = [];
      const region_rank_labels = [];
      const region_rank_name = [];
    
      // console.log("艙位排名");
        axios.post('http://34.125.243.130:5000/region_rank',this.postData)
        .then(res => {
          // handle the response data
          // console.log("進來了");
          // console.log(res);
          for(let i = 0; i < 6; i++) {
            region_rank.push(Number(res.data.rank[i].value)|| 0);
            region_rank_labels.push(res.data.rank[i].rate);
            region_rank_name.push(res.data.rank[i].region);     
          }
          this.region_rank.r1 = region_rank_name[0];
          this.region_rank.r2 = region_rank_name[1];
          this.region_rank.r3 = region_rank_name[2];
          this.region_rank.r4 = region_rank_name[3];
          this.region_rank.r5 = region_rank_name[4];
          this.region_rank.r6 = region_rank_name[5];

          this.regionRankChart.data.labels = [...region_rank_labels];
          this.regionRankChart.data.series = [...region_rank];
          // console.log("class_rank");
          // console.log(this.classRankChart.data);
         
          
          
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    //
  },
  mounted(){
    this.getPastRev("季");
    this.getSalesRate("季");
    this.getClassRank("季");
    this.getRegionRank("季");

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
