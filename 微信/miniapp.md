# 微信小程序的坑

### 微信认证登录

  微信认证登录时必须先 ```wx.login``` ，才能通过执行 ```bindgetphonenumber``` 或 ```bindgetuserinfo``` 获取 ```encryptedData``` 和 ```iv```
 
``` javascript

Page({

  /**
   * 页面的初始数据
   */
  data: {
    code:''
  },

  /**
   * 手机号授权登录
   */
  bindPhoneLogin: function (e) {
    var that = this;
    var code = app.globalData.code;
    var encryptedData = e.detail.encryptedData;
    var iv = e.detail.iv;
    var option = {
      code: encodeURI(this.data.code),
      iv: encodeURI(iv),
      encryptedData: encodeURI(encryptedData)
    }
    if (e.detail.errMsg == 'getPhoneNumber:ok') {
      //微信认证登录
      wx.request({
        url: 'test.php', //仅为示例，并非真实的接口地址
        data: option,
        header: {
          'content-type': 'application/json' // 默认值
        },
        success (res) {
          if (res.data.code == '200') {
            wx.showToast({
              title: '登录成功',
            })
          } else {
            wx.showToast({
              title: '登录失败',
            })
          }
        }
      })
      
    }
  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {
    var that = this;
    //重新获取code
    wx.login({
      success: res => {
        // 发送 res.code 到后台换取 openId, sessionKey, unionId
        if (res.code) {
          console.log(res.code);
          that.setData({
            code: res.code
          })
        }
      }
    })
  },

})

```