<style scoped>
.load-more {
      width: 100%;
      height: 35px;
      line-height: 35px;
      text-align: center;
      background: #f0f0f0;
      color: #888;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
}

.loading {
      display: inline-block;
      width: 20px;
      height: 20px;
      background: url('../img/loading.gif');
      background-size: 20px 20px;
}

.spinner {
      width: 30px;
      height: 30px;
      background-color: #333;
      border-radius: 100%;
      -webkit-animation: sk-scaleout 1.0s infinite ease-in-out;
      animation: sk-scaleout 1.0s infinite ease-in-out;
}

@-webkit-keyframes sk-scaleout {
      0% {
            -webkit-transform: scale(0)
      }
      100% {
            -webkit-transform: scale(1.0);
            opacity: 0;
      }
}

@keyframes sk-scaleout {
      0% {
            -webkit-transform: scale(0);
            transform: scale(0);
      }
      100% {
            -webkit-transform: scale(1.0);
            transform: scale(1.0);
            opacity: 0;
      }
}
</style>
<template>
      <div class="load-more" v-show="show" v-on:click="toggleLoad">
            <span v-show="!isLoading">加载更多···</span>
            <span v-show="isLoading" class="loading"></span>
      </div>
</template>
<script>
import eventHub from '../eventHUb.js'

export default {
      data: function() {
            return {
                  isLoading: false,
                  show: true
            };
      },
      created: function() {
            eventHub.$on("more-loaded", this.toggleLoad);
      },
      methods: {
            toggleLoad: function(e) {
                  if (e && !this.isLoading) {
                        this.isLoading = !this.isLoading;
                        eventHub.$emit("load-more");
                  } else if(!e){
                        this.isLoading = !this.isLoading;
                  }
            }
      }
}
</script>
