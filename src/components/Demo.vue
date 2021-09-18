<template>
  <div class="contain">
    <div class="tab">
      <span v-for="n in 6" :key="n" @click="changePreview(n - 1)"
        >{{ n - 1 }}张预览图</span
      >
      <span @click="allPreview"> all </span>
    </div>
    <div class="wrapper">
      <div class="list">
        <span class="pre" @click="goPreview">pre</span>
        <div class="bs-wrapper">
          <ul class="content">
            <li
              class="item"
              v-for="(item, index) in previews"
              :key="index"
              :style="{
                border:
                  choice_index === index 
                    ? '1px solid #007dff'
                    : '1px solid #e5e5e5',
              }"
              @click="choicePreview(index)"
            >
              <img :src="item" draggable="false" />
            </li>
          </ul>
        </div>
        <span class="next" @click="goNextview">next</span>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from "@better-scroll/core";

export default {
  name: "Demo",
  data() {
    return {
      source_previews: [
        "https://q.kuxaunxuan.cn/image/20210913/6fd0fde98338e64d74c4ae93c1c9bc2a.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/aaaed8561e1a275e165694c4f6c40183.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/172455b9f6c7195c6e407ec6a2d251d9.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/20b53059f1c35052c204f1560a523e54.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/789b94192df5f7a1a966fc322cc870be.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
      ],
      previews: [],
      choice_index: 0,
      bsScroll: undefined,
      itemW:92,
      wrapperW:380,
    };
  },
  watch:{
    choice_index(nv,pv){
      if(!this.bsScroll){
        return;
      }
      console.log("--------------------------");
      console.log("nv="+nv+",pv="+pv);
      if(nv>pv){//往右选择，向左移动
        console.log("scroll.x=" + this.bsScroll.x);
        
        let indexX = this.bsScroll.x + (nv + 1) * this.itemW;
        console.log("indexX="+indexX);

        console.log("wraperW="+this.wrapperW);

        let offsetX = (this.wrapperW - indexX); 
        console.log("offsetX="+offsetX);

        if(offsetX < 0){
          this.bsScroll.scrollTo(this.bsScroll.x + offsetX,0,300);
        }
      }else{//向右移动
        let indexX = this.bsScroll.x + (nv) * this.itemW;
        console.log("indexX="+indexX);

        if(indexX<0){
          this.bsScroll.scrollTo(this.bsScroll.x - indexX,0,300);
        }
      }
    }
  },
  mounted() {
    this.bsScroll = new BScroll(".bs-wrapper", {
      click: true,
      scrollX: true,
      disableMouse: false,
      disableTouch: false,
    });
  },
  methods: {
    changePreview(index) {
      let len = this.source_previews.length;
      this.previews = this.source_previews.slice(len - index);
      this.choice_index = 0;
      if (this.bsScroll) {
        setTimeout(() => {
          this.bsScroll.refresh();
        }, 40);
      }
    },
    choicePreview(index) {
      this.choice_index = index;
    },
    goPreview() {
      this.choice_index =
        this.choice_index - 1 < 0
          ? this.previews.length - 1
          : this.choice_index - 1;
    },
    goNextview() {
      this.choice_index =
        this.choice_index + 1 >= this.previews.length
          ? 0
          : this.choice_index + 1;
    },
    allPreview(){
      this.previews = [
        "https://q.kuxaunxuan.cn/image/20210913/6fd0fde98338e64d74c4ae93c1c9bc2a.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/aaaed8561e1a275e165694c4f6c40183.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/172455b9f6c7195c6e407ec6a2d251d9.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/20b53059f1c35052c204f1560a523e54.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/789b94192df5f7a1a966fc322cc870be.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0", 
        
        "https://q.kuxaunxuan.cn/image/20210913/6fd0fde98338e64d74c4ae93c1c9bc2a.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/aaaed8561e1a275e165694c4f6c40183.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
        "https://q.kuxaunxuan.cn/image/20210916/aaaed8561e1a275e165694c4f6c40183.png?x-oss-process=image/resize,m_mfit,h_80,w_80,limit_0",
      ];

      this.choice_index = 0;
      if (this.bsScroll) {
        setTimeout(() => {
          this.bsScroll.refresh();
        }, 40);
      }
    }
  },
};
</script>

<style lang="scss">
.contain {
  .tab {
    margin-bottom: 40px;
    span {
      margin-right: 20px;
    }
    span:nth-child(2n) {
      border: 1px solid #00ff00;
    }
    span:nth-child(2n + 1) {
      border: 1px solid #ff0000;
    }
  }

  .wrapper {
    .list {
      width: 476px;
      display: flex;
      margin: 0 auto;
      justify-content: space-between;
      span {
        width: 40px;
        border: 1px solid #0000ff;
        user-select:none;
      }

      .bs-wrapper {
        display: inline-block;
        width: 380px;
        height: 86px;
        overflow: hidden;

        .content {
          display: inline-flex;
          padding: 0;
          margin: 0;
          list-style: none;

          li {
            list-style: none;
            width: 80px;
            height: 100%;
            padding: 0;
            margin-right: 10px;
          }
        }
      }
    }
  }
}
</style>
