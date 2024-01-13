<template>
  <card class="card" title="設定參數">
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
              placeholder="日期">
            </date-picker>
          </div>
        </div>

        <div class="row">
          <div class="col-md-12">
            <fg-input
              type="text"
              label="航班編號"
              placeholder="航班編號"
              v-model="para.flight_id"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-12">
            <fg-input
              type="number"
              label="Demand"
              placeholder="Demand"
              v-model="para.demand"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-12">
            <fg-input
              type="number"
              label="未報到率"
              placeholder="未報到率"
              v-model="para.not_show_rate"
            >
            </fg-input>
          </div>
        </div>


        <div class="text-center">
          <!-- @click.native.prevent="updateProfile" -->
          <p-button type="info" round  @click.native.prevent="updateProfile" >
            送出參數
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


export default {
  components: {
      DatePicker,

    },
  data() {
    return {
      para: {
        date: "",
        demand: null,
        not_show_rate: null,
        flight_id: "MM620",
      },
      type: ["", "info", "success", "warning", "danger"],
      notifications: {
        topCenter: false,
      },
    };
  },
  methods: {
    updateProfile() {
      alert("Your data: " + JSON.stringify(this.para));
      const date_string = this.formatDateString(this.para.date);
      this.$emit('childClick', { date: date_string, demand: this.para.demand,  not_show_rate: this.para.not_show_rate});
      console.log(date_string);
    },
    handleClick() {
      // 触发自定义事件
      
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
