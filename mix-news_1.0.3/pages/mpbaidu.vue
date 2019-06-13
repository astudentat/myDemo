<template>
	<div>
		<scroll-view id="nav_ul" class="nav_ul" scroll-x scroll-with-animation :scroll-left="scrollLeft">
			<view 
				v-for="(item,index) in TabList"  :key="item.id"
				class="nav_item"
				:class="{current: index === tabCurrentIndex}"
				:id="'tab'+index"
				@click="changeTab(index)"
			>{{item.name}}</view>
		</scroll-view>
	
		<div>
			<li v-for="item in newsList" :key="item.id">
				{{item.title}}
			</li>
		</div>
	</div>
</template>

<script>
	let windowWidth = 0, scrollTimer = false, tabBar;
	export default {
		data(){
			return{
				tabCurrentIndex: 0, //当前选项卡索引
				scrollLeft: 0, //顶部选项卡左滑距离
				TabList:[],
				newsList:[],
				

				}
		},
		created(){
			this.getU()
		},
		methods:{
			 getU:function(){
                    var that = this;      
                    that.$axios.post('https://www.xiejing.com/mpbaidu.php').then(function(res){  //接口返回数据
                       console.log(JSON.stringify(res));
 					   // console.log((res.data))
						that.TabList=res.data.TabList;                        
						that.newsList=res.data.newsList;                        
                    },function(error){
						console.log(error)
                    })
                }
					
		}
	
	}
</script>

<style lang="scss">
	page, .content{
		background-color: #f8f8f8;
		height: 100%;
		overflow: hidden;
	}
	
	.nav_ul{
		position: relative;
		z-index: 10;
		height: 90upx;
		white-space: nowrap;
		box-shadow: 0 2upx 8upx rgba(0,0,0,.06);
		background-color: #fff;
		.nav_item{
			display: inline-block;
			width: 150upx;
			height: 90upx;
			text-align: center;
			line-height: 90upx;
			font-size: 30upx;
			color: #303133;
			position: relative;
			&:after{
				content: '';
				width: 0;
				height: 0;
				border-bottom: 4upx solid #007aff;
				position: absolute;
				left: 50%;
				bottom: 0;
				transform: translateX(-50%);
				transition: .3s;
			}
		}
	}
</style>
