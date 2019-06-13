<template>
  <div class="city_container">
    <head-top>
      <router-link to="/home" slot="changecity" class="change_city">切换城市</router-link>
    </head-top>
    <form class="city_form">
      <div>
        <input
          type="search"
          name="city"
          id
          placeholder="请输入地址"
          class="city_input input_style"
          v-model="inputValue"
          required
        >
      </div>
      <div>
        <input
          type="submit"
          name="submir"
          id
          class="city_submit input_style"
          @click="postpois"
          value="提交"
        >
      </div>
    </form>
    <header v-if="historyTitle" class="pois_search_history">搜索历史</header>
    <ul class="getpois_ul">
      <li v-for="(item,index) in placelist" @click="nextpage(index,item.geohash)" :key="index">
        <h4 class="pois_name ellipsis">{{item.name}}</h4>
        <p class="pois_address ellipsis">{{item.address}}</p>
      </li>
    </ul>
    <footer v-if="historyTitle&&placelist.length" class="clear_all_history" @click="clearAll">清空所有</footer>
    <div class="search_none_place" v-if="placeNone">无搜索结果</div>
  </div>
</template>

<script>
import headTop from "src/components/header/head";
import { currentcity, searchplace } from "src/service/getData";
import { getStore, setStore, removeStore } from "src/config/mUtils";
export default {
  data() {
    return {
      inputValue: "",
      cityname: "",
      cityid: "",
      placelist: [],
      placeHistory: [],
      historyTitle: "",
      placeNone: false
    };
  },
  components: {
    headTop
  },
  mounted(){
    this.cityid=this.$route.params.cityid;
    currentcity(this.cityid).then(res=>{
      this.cityname=res.name;
    })
    this.initData();
  },
  methods: {
    initData(){
      if(getStore('placeHistory')){
        this.placelist=JSON.parse(getStore('placeHistory'));
      }else{
        this.placelist=[];
      }
    },
    nextpage(index, geohash) {
      let history = getStore("placeHistory");
      let choosePlace = this.placelist[index];
      if (history) {
        let checkrepeat = false;
        this.placeHistory = JSON.parse(history);
        this.placeHistory.forEach(item => {
          if (item.geohash == geohash) {
            checkrepeat = true;
          }
        });
        if (!checkrepeat) {
          this.placeHistory.push(choosePlace);
        }
      } else {
        this.placeHistory.push(choosePlace);
      }
      setStore("placeHistory", this.placeHistory);
      this.$router.push({ path: "/msite", query: { geohash } });
    },
    clearAll() {
      removeStore("placeHistory");
      this.initData();
    },
    postpois(){
      if(this.inputValue){
        searchplace(this.cityid,this.inputValue).then(res=>{
          this.historyTitle=false;
          this.placelist=res;
          this.placeNone=res.length?false:true;
        })
      }
    }
  }
};
</script>

<style lang="scss" scoped>
@import "src/style/mixin";
    .city_container {
      padding-bottom: 2.35rem;
    }
    .change_city {
      right: 0.4rem;
      @include sc(0.6rem, #fff);
      @include ct;
    }
    .city_form {
      background-color: #fff;
      border-top: 1px solid $bc;
      border-bottom: 1px solid $bc;
      padding-bottom: 0.4rem;
      div {
        width: 90%;
        margin: 0 auto;
        text-align: center;
        .input_style {
          border-radius: 0.1rem;
          margin-bottom: 0.4rem;
          @include sc(0.65rem, #fff);
        }
      }
    }
    .pois_search_history{
      border-top: 1px solid $bc;
      border-bottom: 1px solid $bc;
      padding-left: .5rem;
      @include font(.475rem,.8rem);
    }
</style>
