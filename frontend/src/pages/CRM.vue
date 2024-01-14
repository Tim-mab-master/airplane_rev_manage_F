<template>
  <div>


    <!--Charts-->
    <div class="row">
      <!-- 分成左右兩半 -->

        <div class="col-6">
        <chart-card
          title="每期留存率"
          sub-title="Retention Rate"
          :chart-data="retentionChart.data"
          :chart-options="retentionChart.options"
        >
          <!-- <span slot="footer">
            <i class="ti-check"></i> Data information certified
          </span> -->
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> 留存率
          </div>
        </chart-card>
        </div>
        <div class="col-6">
        <chart-card
          title="每期存活率"
          sub-title="Survival Rate"
          :chart-data="survivalChart.data"
          :chart-options="survivalChart.options"
        >
          <!-- <span slot="footer">
            <i class="ti-check"></i> Data information certified
          </span> -->
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> 存活率
          </div>
        </chart-card>
        </div>
      </div>
      <div class="row">
        <div class="col-12">
        <card :title="table1.title">
          <span> 排序依據： </span>
          <select v-model="value" placeholder="請選擇排序依據"  style="border: 0px solid #fffcf5;width:470px;" class="form-control" @change="handleOrderChange">
              <option
                v-for="item in options"
                :key="item"
                :label="item"
                :value="item"           
                >
              </option>
            </select>
          <div slot="raw-content" class="table-responsive">
            <paper-table :data="table1.data" :columns="table1.columns">
            </paper-table>
            <hr/>
          <div class="cus_eq"> Customer Equity: {{ customer_equity }}</div>
          </div>
          
        </card>
      </div>
      </div>
      
      <!-- rfm 表格  -->
      <div class="row">
        <div class="col-12">
        <card :title="table2.title">
          <div slot="raw-content" class="table-responsive">
            <paper-table :data="table2.data" :columns="table2.columns">
            </paper-table>
          </div>
          
        </card>
      </div>
      </div>

     

      
    
  </div>
</template>
<script>
import { StatsCard, ChartCard } from "@/components/index";
// import Chartist from "chartist";
import 'element-ui/lib/theme-chalk/index.css';

import { Option, Select, Button } from 'element-ui';
import { PaperTable } from "@/components";
import axios from 'axios';
const tableColumns = [
    'CustomerID',
    "Customer_name",
    "Gender",
    "PhoneNumber",
    "Birthday",
    "Email",
    "Address",
    "LTV",
    "PCV",
    "RFM",

  ];
  const tableColumns2 = [
    "Classfication",
    "Recency",
    "Frequency",
    "Monetary",
    "Percentage_of_People",
    "Percentage_of_Revenue",
  ];

  // 'CustomerID': 1,
  //       'Customer_name': 'yen chen',
  //       'Gender': 'male',
  //       'PhoneNumber': '0912345678',
  //       'Birthday': datetime.date(2000, 3, 5),
  //       'Email': 'yen@gmail.com',
  //       'Address': 'nccu, wenshang district, Taipei city',
  //       'LTV': 54069.74752117492,
  //       'PCV': 59851.851851851854,
  //       'RFM': '5'
let tableData = [];
let tableData2 = [];
// axios.get('http://34.125.243.130:5000/get_customer_info')
//         .then(res => {
//           // handle the response data
//           for(let i = 0; i < res.data.customer_info.length; i++) {
//                 const inputData = {};
//                 inputData.customerid = res.data.customer_info[i].CustomerID;
//                 inputData.customer_name = res.data.customer_info[i].Customer_name;
//                 inputData.gender = res.data.customer_info[i].Gender;
//                 inputData.phonenumber = res.data.customer_info[i].PhoneNumber;
//                 inputData.birthday = res.data.customer_info[i].Birthday;
//                 inputData.email = res.data.customer_info[i].Email;
//                 inputData.address = res.data.customer_info[i].Address;
//                 inputData.ltv = res.data.customer_info[i].LTV;
//                 inputData.pcv = res.data.customer_info[i].PCV;
//                 inputData.rfm = res.data.customer_info[i].RFM;


//                 tableData.push(inputData);
              
//            }
//            this.table1.data = [...tableData];
         
//           // console.log(res.data);
//           console.log(tableData);
//         })
//         .catch(error => {
//           // handle errors
//           console.error('Error fetching data:', error);
//         });
        let retention_rate = [];
        axios.get('http://34.125.243.130:5000/get_retention_rate')
        .then(res => {
          // handle the response data
          retention_rate = [];
          retention_rate.push(res.data.retention_rate[0]["2023 Q1"]);
          retention_rate.push(res.data.retention_rate[1]["2023 Q2"]);
          retention_rate.push(res.data.retention_rate[2]["2023 Q3"]);
          retention_rate.push(res.data.retention_rate[3]["2023 Q4"]);
          console.log(res.data);
      
          // this.retentionChart.data.series[0] = [...retention_rate];
         
         
          // console.log(res.data);
          console.log();
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
        const survival_rate = [];
        axios.get('http://34.125.243.130:5000/get_survival_rate')
        .then(res => {
          // handle the response data
          survival_rate.push(Number(res.data.survival_rate[0]["2023 Q1"]));
          survival_rate.push(Number(res.data.survival_rate[1]["2023 Q2"]));
          survival_rate.push(Number(res.data.survival_rate[2]["2023 Q3"]));
          survival_rate.push(Number(res.data.survival_rate[3]["2023 Q4"]));
          console.log(survival_rate);
           

          // console.log(res.data);
          // console.log(res.data.survival_rate["2023_Q1"]);
          // console.log(survival_rate);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });

export default {
  components: {
    StatsCard,
    ChartCard,
    PaperTable,
    Option,
    Select,
  },
  /**
   * Chart data used to render stats, charts. Should be replaced with server data
   */
  data() {
    return {
      options: [
       "ID",
       "LTV",
       "PCV",
       
      ],
      value: "ID",
      table1: {
          title: "顧客資訊",
          subTitle: "",
          columns: [...tableColumns],
          data: [...tableData],
        },
      table2: {
          title: "RFM 分析結果",
          subTitle: "",
          columns: [...tableColumns2],
          data: [...tableData2],
      },
      
     
      retentionChart: {
        data: {
          labels: [
            "2023 Q1",
            "2023 Q2",
            "2023 Q3",
            "2023 Q4",
          ],
          series: [
            [...retention_rate],
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
      survivalChart: {
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
      customer_equity: 150000,
      
    };
  },
  methods: {
    handleOrderChange(){
      if(this.value == "LTV"){
        this.fetchData_LTV();
      } else if(this.value == "PCV"){
        this.fetchData_PCV();
      } else{
        this.fetchData_ID();
      }
    },
    fetchData(){
      axios.get('http://34.125.243.130:5000/get_customer_info')
        .then(res => {
          // handle the response data
          tableData = [];
          for(let i = 0; i < res.data.customer_info.length; i++) {
                const inputData = {};
                inputData.customerid = res.data.customer_info[i].CustomerID;
                inputData.customer_name = res.data.customer_info[i].Customer_name;
                inputData.gender = res.data.customer_info[i].Gender;
                inputData.phonenumber = res.data.customer_info[i].PhoneNumber;
                inputData.birthday = res.data.customer_info[i].Birthday;
                inputData.email = res.data.customer_info[i].Email;
                inputData.address = res.data.customer_info[i].Address;
                inputData.ltv = res.data.customer_info[i].LTV;
                inputData.pcv = res.data.customer_info[i].PCV;
                inputData.rfm = res.data.customer_info[i].RFM;


                tableData.push(inputData);
              
           }
           this.table1.data = [...tableData];
         
          // console.log(res.data);
          // console.log(tableData);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    // 載入 RFM 資料
    fetchData_RFM () {
      axios.get('http://34.125.243.130:5000/RFM')
        .then(res => {
          // handle the response data
          tableData2 = [];
         
                let inputData = {};
                inputData.classfication = "黃金客";
                inputData.recency = res.data.RFM_info.import_customer.recency;
                inputData.frequency = res.data.RFM_info.import_customer.frequency;
                inputData.monetary = res.data.RFM_info.import_customer.monetary;
                inputData.percentage_of_people = res.data.RFM_info.import_customer.num_rate;
                inputData.percentage_of_revenue = res.data.RFM_info.import_customer.revenue_rate;

                tableData2.push(inputData);

                inputData = {};
                inputData.classfication = "一般客";
                inputData.recency = res.data.RFM_info.general_customer.recency;
                inputData.frequency = res.data.RFM_info.general_customer.frequency;
                inputData.monetary = res.data.RFM_info.general_customer.monetary;
                inputData.percentage_of_people = res.data.RFM_info.general_customer.num_rate;
                inputData.percentage_of_revenue = res.data.RFM_info.general_customer.revenue_rate;

                tableData2.push(inputData);

                inputData = {};
                inputData.classfication = "沈睡客";
                inputData.recency = res.data.RFM_info.sleeping_customer.recency;
                inputData.frequency = res.data.RFM_info.sleeping_customer.frequency;
                inputData.monetary = res.data.RFM_info.sleeping_customer.monetary;
                inputData.percentage_of_people = res.data.RFM_info.sleeping_customer.num_rate;
                inputData.percentage_of_revenue = res.data.RFM_info.sleeping_customer.revenue_rate;

                tableData2.push(inputData);



           this.table2.data = [...tableData2];
         
          // console.log(res.data);
          // console.log(tableData);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    fetchData_LTV(){
      axios.get('http://34.125.243.130:5000/LTV_order')
        .then(res => {
          // handle the response data
          tableData = [];
          for(let i = 0; i < res.data.cust_info_LTV_Ordered.length; i++) {
                const inputData = {};
                inputData.customerid = res.data.cust_info_LTV_Ordered[i].CustomerID;
                inputData.customer_name = res.data.cust_info_LTV_Ordered[i].Customer_name;
                inputData.gender = res.data.cust_info_LTV_Ordered[i].Gender;
                inputData.phonenumber = res.data.cust_info_LTV_Ordered[i].PhoneNumber;
                inputData.birthday = res.data.cust_info_LTV_Ordered[i].Birthday;
                inputData.email = res.data.cust_info_LTV_Ordered[i].Email;
                inputData.address = res.data.cust_info_LTV_Ordered[i].Address;
                inputData.ltv = res.data.cust_info_LTV_Ordered[i].LTV;
                inputData.pcv = res.data.cust_info_LTV_Ordered[i].PCV;
                inputData.rfm = res.data.cust_info_LTV_Ordered[i].RFM;


                tableData.push(inputData);
              
           }
           this.table1.data = [...tableData];
         
          // console.log(res.data);
          // console.log(tableData);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },
    fetchData_PCV(){
      axios.get('http://34.125.243.130:5000/PCV_order')
        .then(res => {
          // handle the response data
          tableData = [];
          for(let i = 0; i < res.data.cust_info_PCV_Ordered.length; i++) {
                const inputData = {};
                inputData.customerid = res.data.cust_info_PCV_Ordered[i].CustomerID;
                inputData.customer_name = res.data.cust_info_PCV_Ordered[i].Customer_name;
                inputData.gender = res.data.cust_info_PCV_Ordered[i].Gender;
                inputData.phonenumber = res.data.cust_info_PCV_Ordered[i].PhoneNumber;
                inputData.birthday = res.data.cust_info_PCV_Ordered[i].Birthday;
                inputData.email = res.data.cust_info_PCV_Ordered[i].Email;
                inputData.address = res.data.cust_info_PCV_Ordered[i].Address;
                inputData.ltv = res.data.cust_info_PCV_Ordered[i].LTV;
                inputData.pcv = res.data.cust_info_PCV_Ordered[i].PCV;
                inputData.rfm = res.data.cust_info_PCV_Ordered[i].RFM;


                tableData.push(inputData);
              
           }
           this.table1.data = [...tableData];
         
          // console.log(res.data);
          // console.log(tableData);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    fetchData_ID(){
      axios.get('http://34.125.243.130:5000/ID_order')
        .then(res => {
          // handle the response data
          tableData = [];
          for(let i = 0; i < res.data.cust_info_ID_Ordered.length; i++) {
                const inputData = {};
                inputData.customerid = res.data.cust_info_ID_Ordered[i].CustomerID;
                inputData.customer_name = res.data.cust_info_ID_Ordered[i].Customer_name;
                inputData.gender = res.data.cust_info_ID_Ordered[i].Gender;
                inputData.phonenumber = res.data.cust_info_ID_Ordered[i].PhoneNumber;
                inputData.birthday = res.data.cust_info_ID_Ordered[i].Birthday;
                inputData.email = res.data.cust_info_ID_Ordered[i].Email;
                inputData.address = res.data.cust_info_ID_Ordered[i].Address;
                inputData.ltv = res.data.cust_info_ID_Ordered[i].LTV;
                inputData.pcv = res.data.cust_info_ID_Ordered[i].PCV;
                inputData.rfm = res.data.cust_info_ID_Ordered[i].RFM;


                tableData.push(inputData);
              
           }
           this.table1.data = [...tableData];
         
          // console.log(res.data);
          // console.log(tableData);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },

    fetchData2() {
      axios.get('http://34.125.243.130:5000/get_retention_rate')
        .then(res => {
          // handle the response data
          retention_rate = [];
          retention_rate.push(res.data.retention_rate[0]["2023 Q1"]);
          retention_rate.push(res.data.retention_rate[1]["2023 Q2"]);
          retention_rate.push(res.data.retention_rate[2]["2023 Q3"]);
          retention_rate.push(res.data.retention_rate[3]["2023 Q4"]);
          console.log(res.data);
          console.log(res.data.retention_rate[0]["2023 Q1"]);
          console.log(retention_rate);
          console.log("retention_rate");
          // this.retentionChart.data.series[0] = [...retention_rate];
          this.$set(this.retentionChart.data, 'series', [[...retention_rate]]);
         

          console.log(this.retentionChart.data.series[0]);
         
          // console.log(res.data);
          console.log();
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
        // this.retentionChart.data.series[0] = [...retention_rate];
    },

    fetchDataCusEq () {
      axios.get('http://34.125.243.130:5000/CE')
        .then(res => {
          // handle the response data
          
         this.customer_equity = res.data['customer equity'][0]['customer equity'];
          console.log(res.data);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
    },
    
  },
  mounted(){
    this.fetchData();
    // this.fetchData2();
    this.$forceUpdate();
    this.fetchDataCusEq();
    this.fetchData_RFM();
  },
};
</script>
<style lang="scss">
  .cus_eq {
    margin: 10px;
    font-size: 20px;
  }

</style>
