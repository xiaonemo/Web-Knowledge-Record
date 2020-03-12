### git提交代码时报错
##### 解决办法：删除.git文件夹内的hooks文件夹里的git-commit

### 浏览器页签（title）显示“Ant Design Pro”字样
##### 解决办法：修改config/defaultSettings.js中的title

``` javascript
export default {
  navTheme: 'dark',
  // 拂晓蓝
  primaryColor: 'daybreak',
  layout: 'sidemenu',
  contentWidth: 'Fluid',
  fixedHeader: false,
  autoHideHeader: false,
  fixSiderbar: false,
  colorWeak: false,
  menu: {
    locale: true,
  },
  title: '助力出行管理系统',
  pwa: false,
  iconfontUrl: '',
};
```