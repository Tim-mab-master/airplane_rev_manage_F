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
  <hr>
    <div>
      <div class="left_result_title">剩餘機位查詢結果</div>
      <div class="left_result">{{ search_date }} 機位狀況</div>
      <div class="left_result">剩餘機位數量： {{ left_num }}</div>
      <div class="left_result">已預訂機位數量： {{ order_num }}</div>
      <div class="left_result">預訂率： {{ order_rate }}</div>

      

     

      
  
      
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
        postData: {
          flight_code: '',
            date: '',
        },
        //下拉式選單相關
        id: null,
        flight_date: "",
        left_num: null,
        order_num: null,
        order_rate: null,
        search_date: "XXXX/XX/XX",
       
      };
     
    },
    methods: {
        handleCommand(command) {
            console.log("sjlfjasljflasjlfjawlef");
            this.id = String(command);
        },

        formatDateString(inputDateString) {
          // 创建 Date 对象
          const dateObject = new Date(inputDateString);

          // 获取年、月、日
          const year = dateObject.getFullYear();
          const month = String(dateObject.getMonth() + 1).padStart(2, '0'); // 月份从0开始，需要加1
          const day = String(dateObject.getDate()).padStart(2, '0');

          // 格式化为字符串
          const formattedDateString = `${year}-${month}-${day}`;

          return formattedDateString;
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
            this.postData.flight_code = "MM620";
            // 之後記得把日期加上去
            this.postData.date = this.flight_date;
            // this.postData.flight_id = "1";
            // this.postData.date = "2023/1/1";

            console.log( this.postData);
            // console.log(this.postData);
            axios.post('http://34.125.243.130:5000/get_left_seat', this.postData)
            .then(res => {
            // 请求成功时更新数据
              console.log(res.data.date);
              this.search_date = this.formatDateString(res.data.date);
              // this.search_date = this.formatDateString(res.data.date);
              this.left_num = res.data.vacant_num;
              this.order_num = res.data.ordered_num;
              this.order_rate = res.data.ordered_percentage_ratio;
              console.log(res.data);

            })
            .catch(error => {
            // 请求失败时处理错误
                console.error('请求失败：', error);
            });

            
    // ...其他组件方法
     },
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
    .left_result {
      font-size: 20px;
    }
    .left_result_title {
      font-size: 26px;
      font-weight: 400;
      margin: 8px 0px;

    }
  </style>
  