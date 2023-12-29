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
          <div slot="raw-content" class="table-responsive">
            <paper-table :data="table1.data" :columns="table1.columns">
            </paper-table>
            <hr/>
          <div class="cus_eq"> Customer Equity: {{ customer_equity }}</div>
          </div>
          
        </card>
      </div>
      </div>
      

     

      
    
  </div>
</template>
<script>
import { StatsCard, ChartCard } from "@/components/index";
// import Chartist from "chartist";
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
const tableData = [];
axios.get('http://34.125.243.130:5000/get_customer_info')
        .then(res => {
          // handle the response data
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

         
          // console.log(res.data);
          console.log(tableData);
        })
        .catch(error => {
          // handle errors
          console.error('Error fetching data:', error);
        });
        const retention_rate = [];
        axios.get('http://34.125.243.130:5000/get_retention_rate')
        .then(res => {
          // handle the response data
          retention_rate.push(Number(res.data.retention_rate["2023_Q1"])|| 0.05);
          retention_rate.push(Number(res.data.retention_rate["2023_Q2"])|| 0.03);
          retention_rate.push(Number(res.data.retention_rate["2023_Q3"])|| 0.01);
          retention_rate.push(Number(res.data.retention_rate["2023_Q4"])|| 0.02);
          console.log(res.data);
          console.log(res.data.retention_rate["2023_Q1"]);
          console.log(retention_rate);
           

         
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
          survival_rate.push(Number(res.data.survival_rate["2023_Q1"])|| 0.05);
          survival_rate.push(Number(res.data.survival_rate["2023_Q2"])|| 0.03);
          survival_rate.push(Number(res.data.survival_rate["2023_Q3"])|| 0.029);
          survival_rate.push(Number(res.data.survival_rate["2023_Q4"])|| 0.027);
              
           

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
  },
  /**
   * Chart data used to render stats, charts. Should be replaced with server data
   */
  data() {
    return {
      
      table1: {
          title: "顧客資訊",
          subTitle: "",
          columns: [...tableColumns],
          data: [...tableData],
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
    
  },
  mounted(){
    // this.fetchData();
  },
};
</script>
<style lang="scss">
  .cus_eq {
    margin: 10px;
    font-size: 20px;
  }

</style>
