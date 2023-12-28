<template>
    <div>
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

    <date-picker
      v-model="flight_date"
      type="date"
      placeholder="選擇日期">
    </date-picker>

    <!-- @click.native="notifyVue('top', 'center')" -->
    <!-- 按钮 -->
    <button @click="submit"  class="search_button">查詢</button>
  </div>
    <div class="row">
      <div class="col-12">
        <card :title="table1.title">
          <div slot="raw-content" class="table-responsive">
            <paper-table :data="table1.data" :columns="table1.columns">
            </paper-table>
          </div>
        </card>
      </div>

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
  import { PaperTable } from "@/components";
  // 在你的组件中引入需要的 Element UI 组件和样式
  import { Dropdown, DropdownMenu, DropdownItem, DatePicker, Button } from 'element-ui';
  import 'element-ui/lib/theme-chalk/index.css';
  import axios from 'axios';
  import NotificationTemplate from "./Notifications/NotificationOrder";

  // const tableColumns = ["Id", "Name", "Salary", "Country", "City"];
  const tableColumns = [
    "Costomer_Id",
    "Costomer_Name",
    "Flight_Code",
    "Price",
    "Price_Level",
    "Date",
    "From",
    "To",
  ];
  const tableData2 = [
    
  ];
  const tableData = [
   
  ];
  
  export default {
    components: {
      PaperTable,
      Dropdown,
      DropdownMenu,
      DropdownItem,
      DatePicker,
      Button,
    },
    data() {
      return {
        //下拉式選單相關
        id: null,
        flight_date: "",
        table1: {
          title: "訂單查詢",
          subTitle: "Here is a subtitle for this table",
          columns: [...tableColumns],
          data: [...tableData],
        },
        table2: {
          title: "退訂查詢",
          subTitle: "Here is a subtitle for this table",
          columns: [...tableColumns],
          data: [...tableData2],
        },
        postData: {
            flight_id: '',
            date: '',
        },
        type: ["", "info", "success", "warning", "danger"],
        notifications: {
             topCenter: false,
        },
      };
     
    },
    methods: {
        handleCommand(command) {
            console.log("sjlfjasljflasjlfjawlef");
            this.id = String(command);
        },
        // 跳出通知
        notifyVue(verticalAlign, horizontalAlign) {
            const color = Math.floor(Math.random() * 4 + 1);
            this.$notify({
                component: NotificationTemplate,
                icon: "ti-search",
                horizontalAlign: horizontalAlign,
                verticalAlign: verticalAlign,
                type: this.type[color],
            });
        },
        submit() {
            this.postData.flight_id = "1";
            // 之後記得把日期加上去
            this.postData.date = this.flight_date;
            // this.postData.flight_id = "1";
            // this.postData.date = "2023/1/1";

            console.log("24");
            // console.log(this.postData);
            axios.post('http://34.80.114.185:5000/get_order', this.postData)
            .then(res => {
            // 请求成功时更新数据
              this.tableData = [];
              for(let i = 0; i < res.data.orders.length; i++) {
                const inputData = {};
                inputData.costomer_id = res.data.orders[i].CustomerId;
                inputData.costomer_name = res.data.orders[i].CustomerName;
                inputData.flight_code = res.data.orders[i].FlightCode;
                inputData.price = res.data.orders[i].Price;
                inputData.price_level = res.data.orders[i].PriceLevel;
                inputData.date = res.data.orders[i].Date;
                inputData.from = res.data.orders[i].Origin;
                inputData.to = res.data.orders[i].Destination;
                this.tableData.push(inputData);
              }
              this.table1.data = [...this.tableData];
              console.log(this.tableData);
            })
            .catch(error => {
            // 请求失败时处理错误
                console.error('请求失败：', error);
            });

            axios.post('http://34.80.114.185:5000/get_cancel_order', this.postData)
            .then(res => {
            // 请求成功时更新数据
              this.tableData2 = [];
              for(let i = 0; i < res.data.orders.length; i++) {
                const inputData = {};
                inputData.costomer_id = res.data.orders[i].CustomerId;
                inputData.costomer_name = res.data.orders[i].CustomerName;
                inputData.flight_code = res.data.orders[i].FlightCode;
                inputData.price = res.data.orders[i].Price;
                inputData.price_level = res.data.orders[i].PriceLevel;
                inputData.date = res.data.orders[i].Date;
                inputData.from = res.data.orders[i].Origin;
                inputData.to = res.data.orders[i].Destination;
                this.tableData2.push(inputData);
              }
              this.table2.data = [...this.tableData2];
              console.log(this.tableData2);
            })
            .catch(error => {
            // 请求失败时处理错误
                console.error('请求失败：', error);
            });
            },
    // ...其他组件方法
     },
  };
  </script>
  <style lang="scss" >
    .input {
        border-radius: 4px;
        border: 1px solid #DCDFE6;
        padding: 8px 4px;
        margin: 0px 8px 12px 4px;
        padding: 8px 16px;
        text-align: center;
    }
    .search_button {
        // padding: 0.3rem 18px;
        border-radius: 4px;
        border: 1px solid #DCDFE6;
        margin: 0px 8px;
    }
    .el-input__inner {
      border: 1px solid #DCDFE6; 
      height: 40px; 
      background-color: #FFF;
    }
  </style>
  