<template>
  <div class="myWorks">
    <van-loading class="loading-image" v-if="loadingShow" type="spinner" />
    <div class="nothing-tag" v-if="showNoSearch">这个人很懒，什么都没有~</div>
    <div class="recommend-video-content">
        <div 
          v-for="(cell, index) in cellListIdea"
          :key="index"
          :class="(index + 1) % 3==0?'recommend-video-cell-noright':'recommend-video-cell'"
          @click="toDetailPage(cell.type, cell._id)"
        >
          <div class="recommend-video-image">
            <img class="video-cover" :src="cell.idea_img">
            <div class="video-cover-icon"></div>
            <div class="video-tag">{{cell.type | typeFormat}}</div>
          </div>
          <div class="recommend-video-text">
            <div class="recommend-video-title">{{cell.idea_title}}</div>
            <div class="recommend-video-name">
              <img src="../../assets/images/look.png">
              <span>{{cell.read_num}}</span>
              <img class="close-icon" src="../../assets/images/close.png" @click.stop="onDelete(cell._id)">
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import { postFindAuthorIdea } from "@/api/index"
import { postDelIdea } from "@/api/index"
export default {
  name: 'myWorks',
  props: ['author_id'],
  data() {
    return {
      loadingShow: false,
      showNoSearch:false,
      cellListIdea: []
    }
  },
  filters: {
    typeFormat: function (type) {
      if(type == 'article') {
        type = '文章'
        return type
      }
      if(type == 'photo') {
        type = '图片'
        return type
      }
      if(type == 'music') {
        type = '音乐'
        return type
      }
      if(type == 'video') {
        type = '视频'
        return type
      }
    }
  },
  mounted() {
    this.getAuthorIdea()
  },
  methods: {
    getAuthorIdea() {
      this.loadingShow = true
      postFindAuthorIdea({
        author_id: this.author_id
      }).then(res => {
        this.loadingShow = false
        this.cellListIdea = res.resultList.reverse()
        if(this.cellListIdea.length == 0) {
          this.showNoSearch = true
        }
        for(let i = 0; i < this.cellListIdea.length; i++){
          if(this.cellListIdea[i].type == 'photo') {
            this.cellListIdea[i].idea_img = this.cellListIdea[i].idea_images[0]
          }
        }
      })
    },
    onDelete(idea_id) {
      this.$dialog.confirm({
        title: '确定删除此作品吗'
      }).then(() => {
        postDelIdea({
          idea_id: idea_id
        }).then(res => {
          if(res.status == 'ok') {
            this.getAuthorIdea()
            this.$toast('已删除')
          }
        })
      }).catch(() => {
      });
    },
    toDetailPage(type, idea_id) {
      if(type == 'article') {
        this.$router.push({
          name: 'articlePage',
          query: {
            article_id: idea_id
          }
        })
      }
      if(type == 'photo') {
        this.$router.push({
          name: 'photoPage',
          query: {
            photo_id: idea_id
          }
        })
      }
      if(type == 'music') {
        this.$router.push({
          name: 'musicPage',
          query: {
            music_id: idea_id
          }
        })
      }
      if(type == 'video') {
        this.$router.push({
          name: 'videoPage',
          query: {
            video_id: idea_id
          }
        })
      }
    }
  }
}
</script>

<style lang="less" scoped>
.myWorks {
  .loading-image {
    margin: 0 auto;
  }
  .nothing-tag {
    margin-top: 50px;
    text-align: center;
    font-size: 16px;
    color: rgb(202, 202, 202);
  }
  .recommend-video-content {
    padding: 10px 15px;
    .recommend-video-cell {
      width: 30%;
      display: inline-block;
      margin-right: 5%;
      padding-bottom: 5px;
      .recommend-video-image {
        width: 100%;
        height: 100px;
        position: relative;
        border-radius: 5px;
        overflow: hidden;
        .video-cover {
          width: 120%;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
        }
        .video-cover-icon {
          position: absolute;
          right: 0px;
          width: 0;
          height: 0;
          border-top: 40px solid rgb(124, 151, 169);
          border-left: 40px solid transparent;
        }
        .video-tag {
          position: absolute;
          top: 4px;
          right: 2px;
          font-size: 12px;
          color: #FFF;
          transform: rotate(45deg);
        }
      }
      .recommend-video-text {
        margin-top: 5px;
        .recommend-video-title {
          font-size: 14px;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
        }
        .recommend-video-name {
          font-size: 12px;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
          color: rgb(128, 128, 128);
          img {
            width: 10px;
            height: 10px;
            margin-right: 5px;
          }
          .close-icon {
            float: right;
            margin-top: 3px;
            margin-right: 0;
          }
        }
      }
    }
    .recommend-video-cell-noright {
      width: 30%;
      float: right;
      padding-bottom: 5px;
      .recommend-video-image {
        width: 100%;
        height: 100px;
        position: relative;
        border-radius: 5px;
        overflow: hidden;
        .video-cover {
          width: 120%;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
        }
        .video-cover-icon {
          position: absolute;
          right: 0px;
          width: 0;
          height: 0;
          border-top: 40px solid rgb(124, 151, 169);
          border-left: 40px solid transparent;
        }
        .video-tag {
          position: absolute;
          top: 4px;
          right: 2px;
          font-size: 12px;
          color: #FFF;
          transform: rotate(45deg);
        }
      }
      .recommend-video-text {
        margin-top: 5px;
        .recommend-video-title {
          font-size: 14px;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
        }
        .recommend-video-name {
          font-size: 12px;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
          color: rgb(128, 128, 128);
          img {
            width: 10px;
            height: 10px;
            margin-right: 5px;
          }
          .close-icon {
            float: right;
            margin-top: 3px;
            margin-right: 0;
          }
        }
      }
    }
  }
}
</style>
