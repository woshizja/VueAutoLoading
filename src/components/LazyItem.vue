<style scoped>
.lazy-wrap {
      display: block;
      box-sizing: border-box;
      width: 83.3%;
      max-width: 320px;
      height: 371.63px;
      height: ;
      margin: 0 auto;
      border: 1px solid #e1e1e1;
      box-shadow: 1px 1px 5px #bbb;
      border-radius: 4px;
      overflow: hidden;
}

.lazy-wrap .lazy-replace {
      box-sizing: border-box;
      width: 260px;
      height: 260px;
      margin: 20px auto;
      border-radius: 50%;
      background: #ccc;
      color: #fff;
      font-size: 36px;
      font-weight: 600;
      text-align: center;
      display: flex;
      justify-content: center;
      align-items: center;
}

.img-wrap {
      display: block;
      box-sizing: border-box;
      width: 100%;
      max-height: 310.38px;
      overflow: hidden;
      font-size: 0;
      margin: 0 auto;
}

.img-wrap img {
      width: 100%;
}

.img-wrap img.blur {
      filter: blur(5px);
}

.lazy-content {
      overflow: hidden;
}

.lazy-content .desc {
      color: #333;
      font-size: 14px;
      box-sizing: border-box;
      height: 16.7vmin;
      line-height: 1.5;
      padding: 10px 5px;
      margin:0;
}

.desc p {
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
      overflow: hidden;
}

.show-enter-active,
.show-leave-active {
      transition: all 0.3s ease;
}

.show-enter,
.show-leave-active {
      opacity: 0.5;
      transform: scale(0.9);
}
</style>
<template>
      <div class="lazy-wrap">
            <transition name="show">
                  <div class="lazy-content" v-if="itemState!=='hide'">
                        <div class="img-wrap">
                              <img v-bind:src="imgSrc" v-bind:class="{blur:itemState==='small'}">
                        </div>
                        <div class="desc" v-if="itemData.desc">
                              <p>{{itemData.desc}}</p>
                        </div>
                  </div>
            </transition>
            <div v-if="itemState==='hide'" class="lazy-replace">
                  <span>lazy-item</span>
            </div>
      </div>
</template>
<script>
export default {
      props: ['itemData'],
      data: function() {
            return {
                  imgSrc: ''
            }
      },
      computed: {
            itemState: function() {
                  this.showPic(this.itemData.state);
                  return this.itemData.state;
            }
      },
      methods: {
            showPic: function(state) {
                  if (state === "show") {
                        this.imgSrc = this.itemData.smallSrc;
                        var img = new Image();
                        img.onload = () => {
                              this.imgSrc = this.itemData.bigSrc;
                        };
                        img.src = this.itemData.bigSrc;
                  }
            }
      }
}
</script>
