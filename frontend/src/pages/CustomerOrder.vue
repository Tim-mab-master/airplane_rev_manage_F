<template>
  <card class="card" title="乘客訂位系統">
    <div>
      <form @submit.prevent>
        <!-- <div class="row">
          <div class="col-md-12">
            <fg-input
              type="text"
              label="日期"
              placeholder="日期"
              v-model="para.date"
            >
            </fg-input>
          </div>
        </div> -->
        <div class="row">
          <div class="col-md-12">
            <fg-input
              type="text"
              label="出發地"
              placeholder="出發地"
              v-model="para.orgin"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-12">
            <fg-input
              type="text"
              label="目的地"
              placeholder="目的地"
              v-model="para.destination"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-12" style="margin: 0px 0px 16px">
            <label class="control-label">
              日期
            </label>
            <!-- style="border: 1px solid #fffcf5; height: 25px; ackground-color: #fffcf5;" -->
            <date-picker
              style="border: 0px solid #fffcf5;width:470px;"
              class="form-control"
              v-model="para.date"
              type="date"
              placeholder="日期"
              @change="handleDateChange"
              >
            </date-picker>
          </div>
        </div>

       

        <div class="row">
          <div class="col-md-12" style="margin: 0px 0px 16px">
            <label class="control-label">
              艙等與對應價格
            </label>
            <select v-model="value" placeholder="請選擇艙等"  style="border: 0px solid #fffcf5;width:470px;" class="form-control">
              <option
                v-for="item in options"
                :key="item"
                :label="item"
                :value="item"
              
                >
              </option>
            </select>
          </div>
        </div>

      


        <div class="text-center">
          <!-- @click.native.prevent="updateProfile" -->
          <p-button type="info" round  @click.native.prevent="updateProfile" >
            確認送出
          </p-button>
        </div>
        <div class="clearfix"></div>
      </form>
    </div>
  </card>
</template>
<script>
import { DatePicker } from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
// import NotificationTemplate from "./Notifications/NotificationSet";
import { Dropdown, Option, Select,Button } from 'element-ui';
import axios from 'axios';

export default {
  components: {
      DatePicker,
      Option,
      Select,

    },
  data() {
    return {
      options: [
        //  {
        //   // value: '选项2',
        //   label: '双皮奶'
        // },  
       
      ],
        value: '',
        para: {
        date: "",
        flight_id: "MM620",
        orgin: "TPE",
        destination: "NRT",
      },
      depart_date: "",
      type: ["", "info", "success", "warning", "danger"],
      notifications: {
        topCenter: false,
      },
      postData: {
        origin: 'TPE',
        destination: 'NRT',
        department_date: '',
        class: '',

      },
    };
  },
  methods: {
    
    updateProfile() {
      this.postData.department_date = this.para.date;
      // console.log(this.value);
      this.postData.class = this.value.charAt(0);
      console.log(this.postData);
      axios.post('http://34.125.243.130:5000/booking', this.postData)
            .then(res => {
            // 请求成功时更新数据
          
              console.log(res.data);
            })
            .catch(error => {
            // 请求失败时处理错误
                console.error('请求失败：', error);
        });
      
      this.para.date = "";
      this.value = "";
      this.options = [];
      alert("訂位成功");
      
      
      console.log();
    },
    handleDateChange() {
      // 触发自定义事件
      this.depart_date = this.formatDateString(this.para.date);
      axios.post('http://34.125.243.130:5000/booking_flight_info', {depart_date: this.depart_date})
            .then(res => {
            // 请求成功时更新数据
            this.options = [];
              for(let i = 0; i < res.data.class_and_price.length; i++) {
               this.options.push(res.data.class_and_price[i]) ;

              }
             
              console.log(res.data);
            })
            .catch(error => {
            // 请求失败时处理错误
                console.error('请求失败：', error);
            });
           

      
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
  //   notifyVue(verticalAlign, horizontalAlign) {
  //     const color = Math.floor(Math.random() * 4 + 1);
  //     this.$notify({
  //       component: NotificationTemplate,
  //       icon: "ti-settings",
  //       horizontalAlign: horizontalAlign,
  //       verticalAlign: verticalAlign,
  //       type: this.type[color],
  //     });
  //  },
  },
};
</script>
<style lang="scss" scoped>
  .card {
    width: 500px;
    // 看要不要置中
    margin: auto;
  }
  
</style>

<style lang="scss">
  // 用子關係來解決全局問題
  .form-control {
    .el-input__inner {
      border: 1px solid #fffcf5; 
      height: 25px; 
      background-color: #fffcf5;
   }

  }
  
</style>
