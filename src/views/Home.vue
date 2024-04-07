<template>
  <!--轮播图-->
  <el-carousel class="swiper-container" type="card" height="20vw" :interval="4000">
    <el-carousel-item v-for="(item, index) in swiperList" :key="index">
      <img :src="item.picImg" />
    </el-carousel-item>
  </el-carousel>
  <!--热门歌单-->
  <play-list class="play-list-container" title="歌单" path="song-sheet-detail" :playList="songList"></play-list>
  <!--热门歌手-->
  <play-list class="play-list-container" title="歌手" path="singer-detail" :playList="singerList"></play-list>
</template>

<script lang="ts" setup>
import { ref, onMounted } from "vue";
import PlayList from "@/components/PlayList.vue";
import { swiperList, NavName } from "@/enums";
import { HttpManager } from "@/api";
import mixin from "@/mixins/mixin";

// const songList = ref([]); // 歌单列表
// const singerList = ref([]); // 歌手列表
// const { changeIndex } = mixin();
// try {
//   HttpManager.getSongList().then((res) => {
//     songList.value = (res as ResponseBody).data.sort().slice(0,10);
//   });

//   HttpManager.getAllSinger().then((res) => {
//     singerList.value = (res as ResponseBody).data.sort().slice(0, 10);
//   });

//   onMounted(() => {
//     changeIndex(NavName.Home);
//   });
// } catch (error) {
//   console.error(error);
// }
const songList = ref([]); // 歌单列表
const singerList = ref([]); // 歌手列表
const { changeIndex } = mixin();

try {
  // 获取歌单列表
  HttpManager.getSongList().then((res) => {
    const songs = (res as ResponseBody).data;
    const shuffledSongs = shuffleArray(songs);
    songList.value = shuffledSongs.slice(0, 10);
  });

  // 获取歌手列表
  HttpManager.getAllSinger().then((res) => {
    const singers = (res as ResponseBody).data;
    const shuffledSingers = shuffleArray(singers);
    singerList.value = shuffledSingers.slice(0, 10);
  });

  onMounted(() => {
    changeIndex(NavName.Home);
  });
} catch (error) {
  console.error(error);
}

// 无序排列，洗牌shuffle
function shuffleArray(array: any[]) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}

</script>

<style lang="scss" scoped>
@import "@/assets/css/var.scss";

/*轮播图*/
.swiper-container {
  width: 90%;
  margin: auto;
  padding-top: 20px;
  img {
    width: 100%;
  }
}

.swiper-container:deep(.el-carousel__indicators.el-carousel__indicators--outside) {
  display: inline-block;
  transform: translateX(30vw);
}

.el-slider__runway {
  background-color: $color-blue;
}
</style>
