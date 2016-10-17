<template>
      <div id="app">
            <div class="head">
                  <h2>Vue lazy scroll loading</h2>
            </div>
            <div class="descp">
                  <p>基于<b>Vue2.0</b>实现，图片随滚动<b>延迟</b>加载，先请求<b>缩略图</b>，缩略图加载后带模糊效果。滚动到底部后可<b>异步加载</b>更多内容，实现无限加载。</p>
            </div>
            <div v-for="item in items">
                  <lazy-item v-bind:item-data="item"></lazy-item>
                  <i class="divide"></i>
            </div>
            <load-more v-bind:is-loading="isLoading" v-on:click.native="toggleLoad"></load-more>
      </div>
</template>
<script>
import LazyItem from './components/LazyItem.vue'
import LoadMore from './components/LoadMore.vue'
import itemsData from '../data/items.js'

export default {
      data: function() {
            return {
                  items: itemsData,
                  itemsPos: [],
                  timer: null,
                  loader: [],
                  isScrolling: false,
                  isLoading: false
            }
      },
      components: {
            LazyItem,
            LoadMore
      },
      mounted: function() {
            this.initPos(0);
            this.initStates();
            this.bindEvent();
      },
      methods: {
            toggleLoad: function(e) {
                  if (e && !this.isLoading) {
                        this.isLoading = !this.isLoading;
                        this.loadMore();
                  } else if (!e) {
                        this.isLoading = !this.isLoading;
                  }
            },
            loadMore: function() {
                  // 模拟异步加载
                  setTimeout(() => {
                        var tmp = [];
                        for (var i = 0; i < 9; i++) {
                              var t = {};
                              var index = Math.floor(10 * Math.random());
                              for (var key in this.items[index]) {
                                    t[key] = this.items[index][key];
                              }
                              t.state = "hide";
                              tmp.push(t);
                        }
                        this.items = this.items.concat(tmp);
                        this.$nextTick(function() {
                              this.initPos(this.itemsPos.length);
                        });
                        this.toggleLoad();
                  }, 1600);
            },
            initPos: function(start) {
                  var eles = document.querySelectorAll(".lazy-wrap");
                  for (var i = start; i < eles.length; i++) {
                        this.itemsPos.push(eles[i].offsetTop);
                  }
            },
            initStates: function() {
                  this.itemsPos.some((pos, index) => {
                        if (pos >= window.innerHeight) {
                              return true;
                        } else {
                              this.items[index].state = "show";
                        }
                  });
            },
            clearLoader: function() {
                  if (this.loader.length) {
                        this.loader.forEach(v => {
                              clearTimeout(v);
                        });
                        this.loader.length = 0;
                  }
            },
            bindEvent: function() {
                  window.addEventListener("scroll", (e) => {
                        if (this.isScrolling) {
                              // 滚动时清除执行队列队列
                              this.clearLoader();
                              return;
                        } else {
                              this.isScrolling = true;
                              this.timer = setTimeout(() => {
                                    // 滚动停止后（50ms后）检查item
                                    this.clearLoader();
                                    var st = document.body.scrollTop;
                                    this.itemsPos.some((pos, index) => {
                                          if (pos >= st + window.innerHeight - 200) {
                                                return true;
                                          } else if (pos >= st - 200) {
                                                // some是多次调用，每个index的作用域不同
                                                this.loader.push(setTimeout(() => {
                                                      this.items[index].state = "show";
                                                }, 250));
                                          }
                                    });
                                    this.isScrolling = false;
                              }, 250);
                        }
                  });
            }
      }
}
</script>
<style>
body {
      font-family: "PingFang SC", "Hiragino Sans GB", "Helvetica Neue", Roboto, Noto, sans-serif;
      font-size: 12px;
      -webkit-font-smoothing: antialiased;
      text-justify: 100%;
}

* {
      margin: 0;
      padding: 0;
      outline: 0;
      user-select: none;
      -webkit-tap-highlight-color: transparent;
}

.head {
      width: 100%;
      line-height: 50px;
      text-align: center;
      background: #58a;
      color: #fff;
      margin-bottom: 20px;
      box-shadow: 0 1px 5px #888;
}

.descp {
      font-size: 14px;
      color: #333;
      margin-bottom: 20px;
      background: #f0f0f0;
      padding: 8px 10px;
      line-height: 1.5;
}

.divide {
      display: block;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: #ccc;
      position: relative;
      margin: 40px auto;
}

.divide::before {
      content: '';
      position: absolute;
      left: -145px;
      left: -45vw;
      top: 4px;
      display: inline-block;
      width: 145px;
      width: 45vw;
      height: 1px;
      background: linear-gradient( 90deg, #eee, #ccc 40%, #aaa);
}

.divide::after {
      content: '';
      position: absolute;
      left: 8px;
      top: 4px;
      display: inline-block;
      width: 145px;
      width: 45vw;
      height: 1px;
      background: linear-gradient( 90deg, #aaa, #ccc 60%, #eee);
}
</style>
