<template>
  <view>
    <view class="flex-wrp" @click="scanCode">
      <image style="width: 100px; height: 100px; margin-top:20px" mode="aspectFit" src="/static/barcode-scan.png"/>
    </view>
  </view>
</template>

<script>
export default {
  methods: {
    scanCode() {
      console.log("test");
      wx.scanCode({
        success: res => {
          console.log(res);
          const url = "../product/main?barcode=" + res.result;
          wx.navigateTo({ url });
        },
        fail: res => {
          console.log(res);
        }
      });
    }
  }
};
</script>

<style>
.flex-wrp {display: flex; flex-direction:column; justify-content: center; align-items: center}
</style>
