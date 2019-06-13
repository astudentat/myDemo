<template>
    <div class="food_container">
      <head-top :head-title="headTitle" goBack='true'></head-top>
      <section class="sort_container">
        <div class="sort_item" :class="{choose_type:sortBy=='food'}">
          <div class="sort_item_container" @click="chooseType('food')">
            <div class="sort_item_border">
              <span :class="{category_title:sortBy=='food'}">
                {{foodTtile}}
              </span>
              <svg width="10" height="10" xmlns="http://www.w3.org/2000/svg" version="1.1" class="sort_icon">
			    			<polygon points="0,3 10,3 5,8"/>
			    		</svg>
            </div>
          </div>
          <transition name="showlist" v-show="category">
            <section v-show="sortBy=='food'" class="category_container sort_detail_type">
              <section class="category_left">
                <ul>
                  <li v-for="(item,index) in category" :key="index" class="category_left_li" :class="{category_active:restaurant_category_id==item.id}" @click="selectCategoryName(item.id,index)">
                      <section>
                        <img :src="getImgPath(item.image_url)" v-if="index" class="category_icon">
                        <span>{{item.name}}</span>
                      </section>
                      <section>
                        <span class="category_count">
                          {{item.count}}
                        </span>
                        	<svg v-if="index" width="8" height="8" xmlns="http://www.w3.org/2000/svg" version="1.1" class="category_arrow" >
                          	<path d="M0 0 L6 4 L0 8"  stroke="#bbb" stroke-width="1" fill="none"/>
							    		</svg>

                      </section>
                  </li>
                </ul>
              </section>
              <section class="category_right">
                <ul>
                  <li v-for="(item,index) in categoryDetail" v-if="index" :key="index" class="category_right_li" @click="getCategoryIds(item.id,item.name)" :class="{category_right_choosed:restaurant_category_ids==item.id||(!restaurant_category_ids)&&idnex==0}">
                    <span>{{item.name}}</span>
                    <span>{{item.count}}</span>
                  </li>
                </ul>
              </section>
            </section>
          </transition>
      </div>
      <div class="sort_item" :class="{choose_type:sortBy=='sort'}">
        <div class="sort_item_container" @click="chooseType('sort')">
            <div class="sort_item_border">
              <span :class="{category_title:sortBy=='sort'}">排序</span>
              	<svg width="10" height="10" xmlns="http://www.w3.org/2000/svg" version="1.1" class="sort_icon">
			    			<polygon points="0,3 10,3 5,8"/>
			    		</svg>
            </div>
        </div>
        <transition class="showlist">
          <section v-show="sortBy=='sort'" class="sort_detail_type">
            <ul class="sort_list_container" @click="sortList($event)">
              <li class="sort_list_li">
                	<svg>
									<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#default"></use>
								</svg>
                <p data="0" :class="{sort_select:sortByType==0}">
                  <span>智能排序</span>
                  <svg v-if="sortByType==0">
                    	<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                  </svg>
                </p>
              </li>
              <li class="sort_list_li">
                	<svg>
									<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#distance"></use>
								</svg>
	    						<p data="5" :class="{sort_select: sortByType == 5}">
	    							<span>距离最近</span>
	    							<svg v-if="sortByType == 5">
										<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
									</svg>
	    						</p>
              </li>
                      <li class="sort_list_li">
                	<svg>
									<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#distance"></use>
								</svg>
	    						<p data="5" :class="{sort_select: sortByType == 6}">
	    							<span>销量最高</span>
	    							<svg v-if="sortByType == 6">
										<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
									</svg>
	    						</p>
              </li>
               <li class="sort_list_li">
                	<svg>
									<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#default"></use>
								</svg>
                <p data="0" :class="{sort_select:sortByType==1}">
                  <span>起送价最低</span>
                  <svg v-if="sortByType==0">
                    	<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                  </svg>
                </p>
              </li>
              <li class="sort_list_li">
                	<svg>
									<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#distance"></use>
								</svg>
	    						<p data="5" :class="{sort_select: sortByType ==2}">
	    							<span>配送速度最快</span>
	    							<svg v-if="sortByType == 5">
										<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
									</svg>
	    						</p>
              </li>
                      <li class="sort_list_li">
                	<svg>
									<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#distance"></use>
								</svg>
	    						<p data="5" :class="{sort_select: sortByType == 3}">
	    							<span>评分最高</span>
	    							<svg v-if="sortByType == 6">
										<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
									</svg>
	    						</p>
              </li>
            </ul>
          </section>
        </transition>
      </div>
      <div class="sort_item" :class="{choose_type:sortBy=='activity'}" >
        <div class="sort_item_container" @click="chooseType('activity')">
          <span :class="{category_title:sortBy=='activity'}">
            筛选
          </span>
          	<svg width="10" height="10" xmlns="http://www.w3.org/2000/svg" version="1.1" class="sort_icon">
		    			<polygon points="0,3 10,3 5,8"/>
		    		</svg>
        </div>
        <transition name="showlist">
          <section v-show="sortBy=='activity'" class="sort_detail_type filter_container">
            <section style="width:100%">
              <header class="filter_header_style">配送方式</header>
              <ul class="filter_ul">
                <li v-for="(item,index) in Delivery" :key="index" class="filter_li" @click="selectDeliveryMode(item.id)">
                  	<svg :style="{opacity: (item.id == 0)&&(delivery_mode !== 0)? 0: 1}">
										<use xmlns:xlink="http://www.w3.org/1999/xlink" :xlink:href="delivery_mode == item.id? '#selected':'#fengniao'"></use>
									</svg>
                  <span :class="{selected_filter:delivery_mode==item.id}">
                    {{item.text}}
                  </span>
                </li>
              </ul>
            </section>
            <section style="width:100%">
              <header class="filter_header_style">
                  商家属性（可以多选）
              </header>
              <ul class="filter_ul" style="paddingBottom:0.5rem">

                <li v-for="(item,index) in Activity" :key="index" class="filter_li" @click="selectSupportIds(index,item.id)">
                  
                </li>
              </ul>
            </section>
          </section>
        </transition>
      </div>
      </section>
      
    </div>
</template>

<script>
import {mapSate,mapMutations} from 'vuex';
import headTop from "src/components/header/head";
import shopList from 'src/components/common/shoplist';
import {getImgPath} from 'src/components/common/mixin';
import {
  msiteAddress,
  foodCategory,
  foodDelivery,
  foodActivity
} from "src/service/getData";

export default {
data(){
    return{
        geohash:"",
        headTitle:"",
        foodTtile:"",
        restaurant_category_id:"",
        restaurant_category_ids:"",
        sortBy:'',
        category:null,
        sortByType:null,
        Delivery:null,
        Activity:null,
        delivery_mode:null,
        support_ids:[],
        filterNum:0,
        confirmStatus:false
    }
},
created(){
  this.initData();
},
mixins:[getImgPath],
components:{
  headTop,
  shopList
},
methods:{
  async initData(){
    this.geohash=this.$route.query.geohash;
    this.headTitle=this.$route.query.title;
    this.foodTtile=this.headTitle;
    this.restaurant_category_id=this.$route.query.restaurant_category_id;
    if(!this.latitude){
      let res=await msiteAddress(this.geohash);
      this.RECORD_ADDRESS(res);
    }
    this.category=await foodCategory(this.latitude,this.longitude);
    this.category.forEach(item=>{
      if(this.restaurant_category_id==item.id){
        this.categoryDetail=item.sub_categories;
      }
    })
    this.Delivery=await foodDelivery(this.latitude,this.longitude);
    this.Activity=await foodActivity(this.latitude,this.longitude);
    this.Activity.forEach((item,index)=>{
      this.support_ids[index]={status:false,id:item.id};
    })
  },
  async chooseType(type){
    if(this.sortBy!=type){
      this.sortBy=type;
      if(type=='food'){
        this.foodTtile="分类";
      }else{
        this.foodTtile=this.headTitle;
      }
    }else{
      this.sortBy="";
      if(type=='food'){
        this.foodTtile=this.headTitle;
      }

    }
  },
  selectCategoryName(id,index){
    if(index===0){
      this.restaurant_category_ids=null;
      this.sortBy="";
    }else{
      this.restaurant_category_id=id;
      this.categoryDetail=this.category[index].sub_categories;
    }
  },
  getCategoryIds(id,name){
    this.restaurant_category_ids=id;
    this.sortBy="";
    this.foodTtile=this.headTitle=name;
  },
  sortList(event){
    let node;
    if(event.target.nodeName.toUpperCase()=="p"){
      
    }
  },
  selectDeliveryMode(id){
    if(this.delivery_mode==null){
      this.filterNum++;
      this.delivery_mode=id;
    }else if(this.delivery_mode==id){
      this.filterNum--;
      this.delivery_mode==null;
    }else{
      this.delivery_mode=id;
    }
  },
  selectSupportIds(index,id){
    this.support_ids.splice(index,1,{
      status:!status.support_ids[index].status,id
    })
    this.filterNum=this.delivery_mode==null?0:1;
    this.support_ids.forEach(item=>{
      if(item.status){
        this.filterNum++;
      }
    })
  }
}
}
</script>

<style>

</style>
