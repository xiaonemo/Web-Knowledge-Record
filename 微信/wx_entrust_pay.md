# 微信委托支付

## 一、申请委托支付权限，创建模板

![创建模板](https://pay.weixin.qq.com/wiki/doc/api/img/chapter17_2_2.png)

## 二、小程序纯签约

### 小程序签约步骤

- 用户从商户小程序发起签约请求
- 商户将签约请求参数按照规则拼接之后，通过小程序跳转，向签约小程序发起签约请求 
- 用户在微信签约小程序选择支付方式完成签约 
- 微信将签约结果返回给商户 

``` javascript
	wx.navigateToMiniProgram({
    appId:'wxbd687630cd02ce1d',
    path:'pages/index/index',
    extraData:{
      appid:'wxc2626340c63e04b8', //小程序ID
      contract_code: '122',    //签约协议号
      contract_display_account: '张三',  //用户账户展示名称
      mch_id: '1223816102',    //商户号 
      notify_url: 'https://www.qq.com/test/papay',   //回调通知url
      plan_id: '106',    //模板id
      request_serial: '123',   //请求序列号
      timestamp:1414488825,   //时间戳
      sign:'FF1A406564EE701064450CA2149E2514'   //签名
    },
    success(res) {
        // 成功跳转到签约小程序 
    },
    fail(res) {
        // 未成功跳转到签约小程序 
    }
  }) 
```

签约完成后接收返回结果

``` javascript
	App({
    onShow(res) {
      if (res.scene === 1038) { // 场景值1038：从被打开的小程序返回
        const { appId, extraData } = res.referrerInfo
        if (appId == 'wxbd687630cd02ce1d') { // appId为wxbd687630cd02ce1d：从签约小程序跳转回来
          if (typeof extraData == 'undefined'){
              // TODO
              // 客户端小程序不确定签约结果，需要向商户侧后台请求确定签约结果
              return;
          }
          if(extraData.return_code == 'SUCCESS'){
              // TODO
              // 客户端小程序签约成功，需要向商户侧后台请求确认签约结果
              var contract_id = extraData.contract_id
              return;
          } else {
              // TODO
              // 签约失败
              return;
          }
        }
      }
    }
  })
```