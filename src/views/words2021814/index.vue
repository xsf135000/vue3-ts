<template>
  <div class="words">
    <transition name="to-rotate">
      <div class="img-list" v-if="!isRotate">
        <div
          class="img-dom"
          v-for="(item, index) in imgList"
          :key="index"
          :class="{ isSelect: selectIndex === index, disabled: item.isSelect }"
        >
          <img :src="require(`@/assets/images/${index + 1}.png`)" alt="" />
        </div>
      </div>
    </transition>
    <div class="word-text" v-if="isRotate">{{ getWords() }}</div>
    <div
      class="confirm-btn"
      @click="confirm"
      :class="{ disabled: allDisabled() }"
    >
      抽奖
    </div>
  </div>
</template>

<script lang="ts">
import { reactive, toRefs, onMounted } from "vue";

export default {
  components: {},
  setup() {
    const state = reactive({
      selectIndex: -1,
      timer: null as any,
      isRotate: false,
      imgList: Array(0),
    });

    onMounted(() => {
      for (let i = 0; i < 20; i++) {
        state.imgList.push({
          isSelect: false,
        });
      }
    });

    const confirm = () => {
      if (state.timer) {
        return;
      }
      if (state.isRotate) {
        state.isRotate = false;
        setTimeout(() => {
          getInterval();
        }, 1000);
      } else {
        getInterval();
      }
    };

    const getInterval = () => {
      state.timer = setInterval(() => {
        const index = parseInt(String(Math.random() * 20));
        const addIndex = (num: number) => {
          if (state.imgList[num].isSelect) {
            if (num + 1 > 19) {
              addIndex(0);
            } else {
              addIndex(num + 1);
            }
          } else {
            state.selectIndex = num;
          }
        };
        addIndex(index);
      }, 150);
      setTimeout(() => {
        clearInterval(state.timer);
        state.timer = null;
        setTimeout(() => {
          state.imgList[state.selectIndex].isSelect = true;
          state.isRotate = true;
        }, 500);
      }, 3000);
    };

    const getWords = () => {
      let str = "";
      switch (state.selectIndex) {
        case 0:
          str = "你是我唯一的意外";
          break;
        case 1:
          str = "辣鸡憨憨，就知道为难我";
          break;
        case 2:
          str = "困困困困困，变变变变变，一起变国宝吧";
          break;
        case 3:
          str = "天下第一厚脸皮，就是我家憨憨了，看看谁与争锋！";
          break;
        case 4:
          str = "未来里一定有你";
          break;
        case 5:
          str = "你的到来就是七夕最好的礼物";
          break;
        case 6:
          str = "办公室说情话，好羞耻，还要20个满的？不阔能！";
          break;
        case 7:
          str = "这是对我语文最大考验了，我无了";
          break;
        case 9:
          str = "你是我始料不及的遇见，也是我突如其来的欢喜。";
          break;
        case 10:
          str = "困，想抱抱你啦";
          break;
        case 12:
          str = "我会陪你很久，这不是我想，是我会。";
          break;
        case 15:
          str = "大概一切都是命中注定吧";
          break;
        case 16:
          str = "看到你就想笑，就很开心";
          break;
        case 18:
          str = "喜欢你，爱你，想你，每天";
          break;
        case 19:
          str = "一起坐着什么事都不干，也很快乐";
          break;

        default:
          str = "谢谢惠顾";
          break;
      }
      return str;
    };

    const allDisabled = () => {
      let flag = true;
      state.imgList.forEach((item) => {
        if (!item.isSelect) {
          flag = false;
        }
      });

      return flag;
    };

    return {
      ...toRefs(state),
      confirm,
      getInterval,
      getWords,
      allDisabled,
    };
  },
};
</script>
<style scoped lang="less">
div {
  box-sizing: border-box;
}
.words {
  background-color: #fef5ec;
  padding-top: 5rem;
  height: 100vh;
  .img-list {
    width: 20rem;
    height: 25rem;
    overflow: hidden;
    margin: 5rem auto;
    margin-top: 0;
    box-shadow: 0px 0px 4px #ececec;
    .img-dom {
      width: 5rem;
      height: 5rem;
      float: left;
      border: 1px solid #efefef;
      background-color: #fff;
      padding: 1rem;
      &.isSelect {
        background-color: #e6f8ff;
        border: 1px solid #e6f8ff;
      }
      &.disabled {
        background-color: #faf8f8;
      }
      img {
        width: 100%;
        height: 100%;
      }
    }
  }

  .word-text {
    width: 20rem;
    height: 25rem;
    margin: 5rem auto;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ff8981;
  }

  .confirm-btn {
    width: 4.5rem;
    height: 4.5rem;
    border-radius: 50%;
    background-color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    box-shadow: 0px 0px 4px #ececec;
    &.disabled {
      pointer-events: none;
      background-color: #ebebeb;
      color: #fff;
    }
  }

  .to-rotate-enter-active,
  .to-rotate-leave-active {
    transition: opacity 1s, transform 1s;
  }
  .to-rotate-enter,
  .to-rotate-leave-to {
    opacity: 0;
    transform: rotateY(180deg);
  }
}
</style>
