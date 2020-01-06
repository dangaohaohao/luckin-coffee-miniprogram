
<template>
    <div style="width: 750rpx;" :style="{ height: tabHeight + 'rpx', position: 'relative'}" v-if="tabStatus">
        <view class="custom-tabbar"  :style="{ height: tabHeight + 'rpx', backgroundColor: tabBgcolor}" >
            <!-- tabbar title -->
            <scroll-view scroll-with-animation scroll-x :scroll-left="scrollDis" >
                 <div class="tabbar" :style="{
                 	width: tabWidth + 'rpx', height: tabHeight + 'rpx'
                 }">
                    <div :style="{
                    	 lineHeight: tabHeight + 'rpx', 
                    	color: tabIndex == index ? selectFontColor : defaultFontColor,
                      	fontWeight: tabIndex == index ? selectFontWeight : 400,
                        fontSize: tabIndex == index ? selectFontSize + 'rpx' : defaultFontSize + 'rpx'
                      }" 
                      class="tabbar-list" v-for="(item,index) in tabList" :key="index"
                      @click="tabChange($event, index, item.id)">
                        {{item.title}}
                    </div>
                </div>
             </scroll-view>
        </view>
    </div>
    <div v-else="!tabStatus" :style="{width: tabWidth + 'rpx',height: tabHeight + 'rpx', position: 'relative'}">
        <view class="custom-tabbar-vertical"  :style="{width: tabWidth + 'rpx',height: tabHeight + 'rpx', backgroundColor: tabBgcolor}">
            <!-- tabbar title -->
            <scroll-view scroll-with-animation style="height: 100%" scroll-y :scroll-top="scrollDisVertival" >
                 <div class="tabbar-vertical" :style="{
                    width: tabWidth + 'rpx', height: tabListHeight + 'rpx'
                 }">
                    <div :style="{
                         lineHeight: tabListHeight + 'rpx', 
                         height: tabListHeight + 'rpx',
                        color: tabIndex == index ? selectFontColor : defaultFontColor,
                        fontWeight: tabIndex == index ? selectFontWeight : 400,
                        fontSize: tabIndex == index ? selectFontSize + 'rpx' : defaultFontSize + 'rpx'
                      }" 
                      class="tabbar-list-vertical" v-for="(item, index) in tabList" :key="index"
                      @click="tabVerticalChange($event, index, item.id)">
                        {{item.title}}
                    </div>
                </div>
             </scroll-view>
         </view>
     </div>
</template>
<script>

    export default {
		name: 'custom-tabbar',
        data() {
            return{
                tabIndex: 0,               // 当前显示点击位置
                tabSecondIndex: 0,         // 第二位置
                scrollDis: 0,              // 横向滚动条位置
                scrollDisVertival: 0,      // 纵向滚动条位置
            }
        },
        props: {
        	// tabbar 状态 => false纵向 or true横向
        	tabStatus: {
        		type: Boolean,
        		default: true
        	},
        	// tabbar 数组
        	tabList: {
        		type: Array,
        		default: []
        	},
            tabBgcolor: {
                type: String,
                default: "#ffffff"
            },
        	// 整个 tabbar 宽度
        	tabWidth: {
        		type: Number,
        		default: 750
        	},
        	// 整个 tabbar 高度
        	tabHeight: {
        		type: Number,
        		default: 1334
        	},
            // 单项宽度
            tabListWidth: {
                type: Number,
                default: 200
            },
            // 单项高度
            tabListHeight: {
                type: Number,
                default: 100
            },
        	// 单项没选中的默认字体色
        	defaultFontColor: {
				type: String,
				default: '#333333'
        	},
        	// 单项 选中颜色
        	selectFontColor: {
        		type: String,
        		default: '#f8643c'
        	},
        	// 选中加粗
        	selectFontWeight: {
        		type: Number,
        		default: 400
        	},
            // 默认字号
            defaultFontSize: {
                type: Number,
                default: 32
            },
            // 选中字号
            selectFontSize: {
                type: Number,
                default: 32
            },
            // 是否启用二级纵向  启用=>true
            tabbarSecondVertical: {
                type: Boolean,
                default: false
            }
        },
        methods: {
            //  横向  tabbar改变
            tabChange(e, val, id) { 
            	e = e ? e : event
                // 理想宽度
                let offsetWidth = this.tabWidth > 750 ? 750 : this.tabWidth
                // 盾滚动宽度
                let offsetLeft = e.currentTarget.offsetLeft

            	// 判断点击坐标是否过半平 过了动 不过不动
                if (uni.upx2px(offsetWidth / 2) > offsetLeft) {
                    this.scrollDis = 0
                } else {
                    let dis = offsetLeft - uni.upx2px(offsetWidth / 2)  
                    this.scrollDis = dis
                }
                this.tabIndex = val
                this.$emit("tabClick", {
                    id: id,
                    index: val
                })
            },
            tabVerticalChange(e, val, id) {
                e = e ? e : event
                console.log(e)
                // 理想高度
                let offsetHeight = this.tabHeight > 1334 ? 1334 : this.tabHeight
                // 滚动高度
                let offsetTop = e.currentTarget.offsetTop

                // 判断点击坐标是否过半平 过了动 不过不动
                if(uni.upx2px(offsetHeight / 2) > offsetTop){
                    this.scrollDisVertival = 0
                } else {
                    let dis = offsetTop - uni.upx2px(offsetHeight / 2)
                    this.scrollDisVertival = dis
                }
                this.tabIndex = val
                console.log(id)
                this.$emit("btnClick", {
                    id: id,
                    index: val
                })
            },
            tabbarSecondVerticalChange(e, val) {
                e = e ? e : event
                this.tabSecondIndex = val
            } 
        },
        created() {
        }
    }
</script>

<style>
    .custom-tabbar {
        border-bottom: 1rpx solid #bbb;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        z-index: 10001;
        overflow: hidden;
        }
        .tabbar {
            display: flex;
            flex-wrap: nowrap;
            justify-content: space-around;
        }
        .tabbar-list {
            height: 100%;
            text-align: center;
            font-size: 28rpx;
        }
   .custom-tabbar-vertical {
        border-right: 1rpx solid #eee;
        background-color: #fff;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 10001;
        overflow: hidden;
       }
       .tabbar-vertical {
            
           }
           .tabbar-list-vertical {
                width: 100%;
                text-align: center;
           }

</style>
