## npm常用命令

### npm install 安装模块

#### -S, --save 安装包信息将加入到dependencies（生产阶段的依赖）
#### -D, --save-dev 安装包信息将加入到devDependencies（开发阶段的依赖），所以开发阶段一般使用它
#### -O, --save-optional 安装包信息将加入到optionalDependencies（可选阶段的依赖）
#### -E, --save-exact 精确安装指定模块版本
#### -g 全局安装
#### npm install默认 本地安装

### npm uninstall 卸载模块 
### npm update 更新模块
### npm outdated 检查模块是否已经过时
### npm ls 查看安装的模块
### npm init 在项目中引导创建一个package.json文件
### npm help 查看某条命令的详细帮助 
### npm root 查看包的安装路径
### npm config 管理npm的配置路径
### npm cache 管理模块的缓存
#### npm cache clean 清除本地缓存
#### npm cache clear --force

### npm start 启动模块
### npm stop 停止模块
### npm restart 重新启动模块
### npm test 测试模块
### npm version 查看模块版本
### npm view 查看模块的注册信息
### npm adduser 用户登录
### npm publish 发布模块
### npm access 在发布的包上设置访问级别