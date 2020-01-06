# 自定义TABBAR ()

## 横向

### 注意事项
            全局的宽度高度尺寸以 2倍设计图 750 | 1334 为基准
            tabWidth范围 =>  tabWidth > 750
     横向需注意 为了美感上的差距, 需要tabWidth(tab宽度)大于750, 在足够的范围内展示tabList(菜单)时,
      tabWidth不用设置默认750就可以, 当750无法满足屏幕时, 需要根据实际情况调整tabWidth > 750 ++
      例如 2000才能展示所有内容设置 tabWidth = 2000
     -效果-
      ![30菜单 3000](http://hot.dev.mengjing.site/image/tab3000.png)

#### Attributes 属性

| Attributes | tabStatus           | tabList          | tabBgcolor | tabWidth | tabHeight   | selectFontWeight  |
| :--------: | :-----------------: | :-----:          | :--------: | :------: | :---------: | :---------------: |
| 含义       | tab 状态             | tab菜单          | tab 背景色  | tab 宽度 | tab高度      | 选中加粗          |
| value      | true(横) / false(纵) | Array           | String      | Number  | Number       |  Number           |
| 默认       | false (横向)         | [{id:0,title:"title"}] | "#ffffff" | 750 | 1334         |  400(正常100-700) |
| 必填       | 否                   | 是              | 否         | 否       | 横向还是设置把 | 否               |

| Attributes | defaultFontColor    | selectFontColor | defaultFontSize | selectFontSize | 
| :--------: | :-----------------: | :-------------: | :-------------: | :------------: | 
| 含义       | 默认字体色           | 选中字体色       |  默认体字大小    | 选中字体大小    | 
| value      |    String           | String          | Number          | Number         |
| 默认       | "#333333" rgb        | '#f8643c'      |  32             |  32             |
| 必填       | 否                   | 否              | 否              |  否             | 

#### methods 方法
    
    <template>
      <custom-tabbar  :tabList="tabbarList" :tabWidth="750" :tabHeight="100" @tabClick="tabclick"></custom-tabbar>
    </template>
    <script>
        import customTabbar from '@/components/custom-tabbar/custom-tabbar.vue'
        export default {
            components: {customTabbar},
            data() {
                return{
                    tabbarList: [
                        {id: 0, title: "哈哈"},
                        {id: 1, title: "嘻嘻"},
                        {id: 2, title: "嘿嘿"},
                        {id: 3, title: "哒哒"},
                        {id: 4, title: "hiahia"},
                    ]  
                }
            },
            methods: {
                tabclick(val) {
                    console.log(val)
                    // 返回格式
                    {
                        id: 1,      // 对应id
                        index: 1    // 对应数组索引
                    }
                },
            },
        }
    </script>



## 纵向

### 纵向一级tabbar
#### 注意事项
        tabHeight => tab高度设置不超过1334就ok了, 超过也没什么, 正常范围 0 < tabHeight < 1334
        解决布局问题 
=> 左tab 右内容
    -外边-套个盒子flex布局
    ![布局 例如](http://hot.dev.mengjing.site/image/tabVerticalDemo.png)
    
    <view  style="width: 750rpx; height: 1334rpx;display: flex">
        <custom-tabbar defaultFontColor="#bbbbbb" selectFontColor="#00ffff" :tabList="tabbarList"
          :tabWidth="200" :tabHeight="1334" :tabStatus="false"
          :selectFontSize="40" :defaultFontSize="32" tabBgcolor="orangered"
          @tabClick="btnclick">
        </custom-tabbar>
        <div>噶哈哈哈哈</div>
    </view>

#### Attributes 属性

| Attributes | tabStatus           | tabList          | tabBgcolor | tabWidth | tabHeight   | selectFontWeight  |
| :--------: | :-----------------: | :-----:          | :--------: | :------: | :---------: | :---------------: |
| 含义       | tab 状态             | tab菜单          | tab 背景色  | tab 宽度 | tab高度      | 选中加粗          |
| value      | true(横) / false(纵) | Array           | String      | Number  | Number       |  Number           |
| 默认       | true (纵向)         | [{id:0,title:"title"}] | "#ffffff"  | 750      | 1334         |  400(正常100-700) |
| 必填       | 否                   | 是              | 否         | 否       | 横向还是设置把 | 否               |


| Attributes | tabListHeight | defaultFontColor    | selectFontColor | defaultFontSize | selectFontSize | 
| :--------: | :-----------: | :-----------------: | :-------------: | :-------------: | :------------: | 
| 含义       | 子元素高度     | 默认字体色           | 选中字体色       |  默认体字大小    | 选中字体大小    | 
| value      | Number        |    String           | String          | Number          | Number         |
| 默认       | 100            | "#333333" rgb        | '#f8643c'      |  32             |  32             |
| 必填       | 否             | 否                   | 否              | 否              |  否             | 

#### methods 方法
    
    <template>
      <custom-tabbar :tabStatus="false" :tabList="tabbarList" :tabWidth="750" :tabHeight="100" @tabClick="tabclick"></custom-tabbar>
    </template>
    <script>
        import customTabbar from '@/components/custom-tabbar/custom-tabbar.vue'
        export default {
            components: {customTabbar},
            data() {
                return{
                    tabbarList: [
                        {id: 0, title: "哈哈"},
                        {id: 1, title: "嘻嘻"},
                        {id: 2, title: "嘿嘿"},
                        {id: 3, title: "哒哒"},
                        {id: 4, title: "hiahia"},
                    ]  
                }
            },
            methods: {
                tabclick(val) {
                    console.log(val)
                    // 返回格式
                    {
                        id: 1,      // 对应id
                        index: 1    // 对应数组索引
                    }
                },
            },
        }
    </script>
### 纵向二级tabbar
<二次  待更新>