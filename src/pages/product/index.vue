<template>
    <view>
        <view>
            <view class="rich-text-wrp">
              <rich-text :nodes="nodes"></rich-text>
            </view>
            <!-- <image style="width: 100px; height: 100px; margin-top:20px" mode="aspectFit" :src="image"/> -->
        </view>
    </view>
</template>

<script>
export default {
  data() {
    return {
      nodes: [],
      image: ""
    };
  },
  methods: {
    getProduct() {
      const url =
        "https://git.maxsns.com/wechat/product?barcode=" +
        this.$root.$mp.query.barcode;
      console.log(url);
      console.log(this.test + " before ");
      const that = this;
      const requestTask = wx.request({
        url: url,
        header: {
          "content-type": "text/html"
        },
        success: function(res) {
          console.log(res);
          if (res.statusCode == 200) {
            const data = res.data;
            for (const key in data) {
              if (data.hasOwnProperty(key)) {
                const element = data[key];
                let node = {
                  name: "div",
                  attrs: {
                    class: "div_class",
                    style: "line-height: 30px;"
                  }
                };
                switch (key) {
                  case "name":
                    node["children"] = [
                      {
                        type: "text",
                        text: "名称："
                      },
                      {
                        type: "text",
                        text: data["name"]
                      }
                    ];
                    break;
                  case "barcode":
                    node["children"] = [
                      {
                        type: "text",
                        text: "条码："
                      },
                      {
                        type: "text",
                        text: data["barcode"]
                      }
                    ];
                    break;
                  case "default_code":
                    node["children"] = [
                      {
                        type: "text",
                        text: "内部参照："
                      },
                      {
                        type: "text",
                        text: data["default_code"]
                      }
                    ];
                    break;
                  case "weight":
                    node["children"] = [
                      {
                        type: "text",
                        text: "重量："
                      },
                      {
                        type: "text",
                        text: data["weight"].toString()
                      }
                    ];
                    break;
                  case "type":
                    node["children"] = [
                      {
                        type: "text",
                        text: "产品类型："
                      },
                      {
                        type: "text",
                        text: data["type"]
                      }
                    ];
                    break;
                  case "list_price":
                    node["children"] = [
                      {
                        type: "text",
                        text: "销售价格："
                      },
                      {
                        type: "text",
                        text: data["list_price"].toString()
                      }
                    ];
                    break;
                  case "responsible_id":
                    node["children"] = [
                      {
                        type: "text",
                        text: "负责："
                      },
                      {
                        type: "text",
                        text: data["responsible_id"][1]
                      }
                    ];
                    break;
                  case "virtual_available":
                    node["children"] = [
                      {
                        type: "text",
                        text: "本地库存："
                      },
                      {
                        type: "text",
                        text: data["virtual_available"].toString()
                      }
                    ];
                    break;
                  case "property_stock_production":
                    node["children"] = [
                      {
                        type: "text",
                        text: "生产位置："
                      },
                      {
                        type: "text",
                        text: data["property_stock_production"][1]
                      }
                    ];
                    break;
                  case "property_stock_inventory":
                    node["children"] = [
                      {
                        type: "text",
                        text: "库存位置："
                      },
                      {
                        type: "text",
                        text: data["property_stock_inventory"][1]
                      }
                    ];
                    break;
                  case "image_medium":
                    that.image = data["image_medium"];
                    break;
                  default:
                    break;
                }
                that.nodes.push(node);
              }
            }
          }
        },
        fail: function(err) {
          console.log(err);
        }
      });
      // requestTask.abort();
    }
  },
  created() {
    console.log("created");
  },
  onShow() {
    console.log(this.$root.$mp.query.barcode);
    this.getProduct();
  },
  onHide() {
    console.log("onHide");
  },
  onUnload() {
    console.log("onUnload");
    this.nodes = [];
  }
};
</script>

<style>
.rich-text-wrp {display: flex; flex-direction:column; justify-content: center; align-items: center}
</style>