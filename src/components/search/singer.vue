<template>
  <div class="artistsContainer">
    <div v-if="getDataFinish" style="width:100%;">
      <div @click="singerSongList(item.id)"  v-for="(item,index) in artists" :key="index" class="artistItem">
        <el-image shape="square"  :src="item.picUrl">
          <div slot="placeholder" class="image-slot">
            <i class="el-icon-loading"></i>
            <p>加载中</p>
          </div>
        </el-image>
        <p style="font-size:13px;">{{item.name}}</p>
      </div>
    </div>
    <div v-else style="text-align:center;width:100%;">
      <i class="el-icon-loading"></i>
      <p>加载中</p>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      artists: [],
      getDataFinish: false
    };
  },
  created(){
 this.isLogin();
  },
  mounted() {
    const search = this.$store.state.search;
   
    this.$axios
      .get("http://zhangpengfan.xyz:3000/search?keywords=" + search.keywords + "&type=" + search.select)
      .then(res => {
       
        this.artists = this.artists.concat(res.data.result.artists);
       this.getDataFinish = true;
      })
      .catch(err => {
        console.log(err);
      });
  },
  methods:{
    singerSongList(e){
      this.$store.commit('getSingerId',e);
      this.$store.commit('goToPreviousPage','search')
      this.$router.push('/singersongs');
    }
  }
};
</script>
<style scoped>
.artistsContainer {
  width: 96%;
  margin: 0 2%;
  left: 0;
  display: flex;
  flex-wrap: wrap;
}
.artistItem {   
  width: 100%;
  margin:10px 0;
  border-bottom: 1px solid #bdbdbd;
}
</style>