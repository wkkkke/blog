<template>
  <div id="app" class="app">
    <page :currentPage="currentPage">
      <img class="avatar" src="./assets/2.jpeg" alt="头像">
      <section class="animate" ref="section1">
      
      </section>
    </page>
    <page :currentPage="currentPage">
     
      <section class="animate move-left">
      
           
           
      </section>
    </page>
    <page :currentPage="currentPage">
      <h1 class="text-center">方法说明</h1>
      <section class="animate move-left">
        <p>在每个options的对象中，可以设置两种方法：beforeLeave 和 afterEnter</p>
        <ul>
          <li>beforeLeave 方法表示在离开当前页面前所做的操作</li>
          <li>afterEnter 方法表示在进入当前页面后所做的操作</li>
        </ul>
        <p>这两个方法都有一个默认参数，该参数为当前Page的vue组件实例，方法的this为App.vue的组件实例</p>
      </section>
    </page>
    <page :currentPage="currentPage">
     
      <section class="animate move-left">
       
       
      </section>
    </page>
    <page-controller :pageNum="pageNum" :currentPage="currentPage" @changePage="changePage" :option="controllerOption"></page-controller>
  </div>
</template>

<script>
import Page from './components/Page.vue';
import PageController from './components/PageController.vue';

// 页面进出动画
function afterEnterAnimate($child) {
  $child.$el.querySelector('.animate').classList.remove('move-left', 'move-right');
}
function beforeLeaveAnimate($child) {
  let moveType = Math.random() > 0.5 ? 'move-left' : 'move-right';
  $child.$el.querySelector('.animate').classList.add(moveType);
}

export default {
  name: 'app',
  data() {
    return {
      currentPage: 1,
      options: [{
        // the color of background
        background: 'rgb(140, 252, 252)',
        // the color of text
        color: '#fff',
        // is content center
        isCenter: true,
        // the function before page show
        afterEnter: afterEnterAnimate,
        // the function after page show
        beforeLeave: beforeLeaveAnimate
      }, {
        background: ' rgb(240, 181, 181)',
        color: '#fff',
        isCenter: true,
        afterEnter: afterEnterAnimate,
        beforeLeave: beforeLeaveAnimate
      }, {
        background: 'rgb(155, 156, 245)',
        color: '#fff',
        isCenter: true,
        afterEnter: afterEnterAnimate,
        beforeLeave: beforeLeaveAnimate
      }, {
        background: 'rgb(192, 133, 184)',
        color: '#fff',
        isCenter: true,
        afterEnter: afterEnterAnimate,
        beforeLeave: beforeLeaveAnimate
      }],
      controllerOption: {
        arrowsType: false,
        navbar: true,
        highlight: true,
        loop: false
      }
    }
  },
  computed: {
    // 总page数
    pageNum() {
      return this.options.length;
    }
  },
  methods: {
    changePage(index) {
      // beforeLeave Hook
      let beforeIndex = this.currentPage - 1;
      let leaveFunction = this.options[beforeIndex].beforeLeave;
      typeof leaveFunction === 'function' && leaveFunction.call(this, this.$children[beforeIndex]);
      // change page
      this.currentPage = index;
      // afterEnter Hook
      let nextIndex = index - 1;
      let enterFunction = this.options[nextIndex].afterEnter;
      this.$nextTick(function() {
        typeof enterFunction === 'function' && enterFunction.call(this, this.$children[nextIndex]);
      })
    }
  },
  components: {
    Page, PageController
  },
  mounted() {
    this.$children.forEach((child, index) => {
      // 动态设置各个page内的options
      if (child.option === null) {
        let childOption = this.options[index];
        this.$set(childOption, 'index', index + 1);
        child.option = childOption;
      }
    });
  }
}
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  overflow: hidden;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
}

.app {
  height: 100%;
  width: 100%;
}



/* 下面的是与fullPage无关的样式 */

.animate {
  transition: all 1s ease-out 0s;
}

.move-left {
  transform: translateX(-1000%);
}

.move-right {
  transform: translateX(1000%);
}

@media screen and (max-width:768px) {
  html,
  body {
    font-size: 12px;
  }
}

a {
  text-decoration: none;
  color: inherit;
}

a:hover {
  text-decoration: underline;
}

.person-img {
  width: 223px;
  height: 185px;
  float: left;
  background-color: #fff;
  box-shadow: 3px 3px 10px #999;
}

.person-img img {
  height: 100%;
  width: 100%;
}

.person-basic-info {
  overflow: hidden;
  float: right;
  margin-left: 40px;
}

.info-line {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
}

.text-center {
  text-align: center;
}

.text-bold {
  font-weight: bold;
}

.demo-intro {
  text-indent: 2em;
}

dt {
  font-weight: bold;
  font-size: 16px;
}

ul {
  padding-left: 1em;
}

.avatar {
  margin: 10px auto;
  display: block;
  box-shadow: 10px 10px 20px rgb(224, 172, 172);
  border-radius: 50%;
  width:168px;
  height:168px;
}

.author-info {
  text-align: center;
}
</style>
