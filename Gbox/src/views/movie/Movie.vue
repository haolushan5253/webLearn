<template>
    <div>
       <ul>
        <li @click="gotoDetail(movie.id)" v-for="movie in movieList" :key="movie.id" class="movie">
              <div class="movie-img">
                <img :src="movie.images.large" alt="">
            </div>  
            <div class="movie-info">
                <div class="movie-info-title">{{movie.title}}</div>
                <div>观众评 <span class="movie-info-average">{{movie.rating.average}}</span></div>
                <div class="movie-info-star">主演：
                    <span v-for="item in movie.casts" :key="item.id">
                        {{item.name}}&nbsp;
                    </span>
                </div>
            </div>  
            
        </li>
    </ul> 
    <div class="end" v-show="isEnd">
            <h3>数据到底了</h3>
        </div>
        <div class="loading" v-show="isLoading">
            <img src="@/assets/img/loading.gif" alt="">
        </div>
    </div>
    
</template>



<script>
import axios from "axios";
export default {
  data() {
    return {
      movieList: [],
      isLoading: true,
      isEnd: false
    };
  },
  methods: {
    getData() {
      // let url1 =
      //   "https://bird.ioliu.cn/v2?url=https://api.douban.com/v2/movie/top250?start=0&count=5";
      // let url2 = "https://api.myjson.com/bins/pb8vw";
      let url3 = "https://bird.ioliu.cn/v2?url=https://api.douban.com/v2/movie/in_theaters?start=0&count=5";
      let url4 = "https://api.myjson.com/bins/lk966";
      this.isLoading = true;
      axios.get(url4).then(res => {
        // 由于接口不能用了，不得已采用假分页
        let getList = res.data.subjects.slice(
          this.movieList.length,
          this.movieList.length + 5
        );
        if (getList.length < 5) {
          this.isEnd = true;
        }
        this.movieList = this.movieList.concat(getList);
        this.isLoading = false;
      });
    },
    gotoDetail(movieId){
      this.$router.push(`/moviedetail/${movieId}`);
    }
  },
  created() {
    this.$emit("switchTab", "movie");
    this.getData();
  },
  mounted() {
    window.onscroll = () => {
      let scrollTop = document.documentElement.scrollTop;
      let scrollHeight = document.documentElement.scrollHeight;
      let clientHeight = document.documentElement.clientHeight;
      // console.log(scrollHeight, scrollTop, clientHeight);
      if (scrollHeight == scrollTop + clientHeight && !this.isEnd) {
        // 请求数据
        this.getData();
      }
    };
  }
};

    //     created () {
    //         let url1 = "http://api.myjson.com/bins/pb8vw";
    //         let url2 ="https://bird.ioliu.cn/v2?url=https://api.douban.com/v2/movie/top250?";
    //         axios.get(url1).then(res => {
    //             this.movieList =  res.data.subjects;
    //         })
    //     }

    // }

</script>

<style lang="scss" scoped>
.movie{
    display: flex;
    padding: 0.2rem;
    border-bottom: 0.02rem solid #ccc;
    &-img{
        flex-grow: 1;
        width: 0;
        img{
            width: 100%;
        }

    }
    &-info{
        flex-grow: 3;
        width: 0;
        margin-left: 0.2rem;
        &-title{
            color: #333;
            font-size: 0.34rem;
            font-weight: 700;
        }
        &-average{
            font-weight: 700;
            color: #faaf00;
        }
        &-star{
            color:#666;
            font-size: 0.26rem;
        }
    } 
}
.loading {
  text-align: center;
  position: fixed;
  bottom: 1rem;
  width: 100%;
  img {
    width: 1rem;
    height: 1rem;
  }
}

.end {
  text-align: center;
}
</style>
