<template>
<!-- 选择logo组件 -->
    <div class="main-logo">
        <!-- v-cloak 表示刷新页面是让vue组件隐藏，而不显示一闪而过的{{代码}}-->
        <img :src="logoData[selectedNow].imgSrc" v-cloak>
        <!-- @是v-on的缩写，v-on是绑定事件的指令 -->
        <span class="logoList-arrow" @click="toggleFlag"></span>
        <!-- tranition是vue过渡,本案例是vue2.0的过渡写法 -->
        <transition name="logofade" v-cloak>
            <ul v-show="logoListFlag" class="logoList">
                <!-- vue2.0中v-for指令index写在logo后面,vue2.0以前是index写在logo前面,本案例是vue2.0 -->
                <li v-for="(logo,index) in logoData" class="logoItem" @mouseover="logoListHover(index)" :class="{selectback:index==logoNow}" @click="logoSelected(index)">
                    <img :src="logo.imgSrc">
                </li>
            </ul>
        </transition>
    </div>
</template>

<script>
// export defult是es6中的方法，将整个花括号对象暴露出去;
export default {
    data: function() {
        return {
            selectedNow: 0,
            logoNow:-1,
            logoListFlag: false,
            //静态资源放入static文件夹中
            logoData:[{imgSrc:'../../static/images/360_logo.png'},{imgSrc:'../../static/images/baidu_logo.png'},{imgSrc:'../../static/images/sougou_logo.png'}]
        }
    },
    methods: {
        toggleFlag: function() {
            this.logoListFlag = !this.logoListFlag;
        },
        logoListHover: function(index) {
            this.logoNow = index;
        },
        logoSelected: function(index) {
            this.selectedNow = index;
            this.logoListFlag = false;
            // 触发父组件的自定义事件，向父组件传参数,selectNow是选择了哪个搜索引擎的索引，父组件得到了之后就可以指定搜索时跳转到哪个搜索引擎。父子组件通信可以看vue文档 组件那一章
            this.$emit('getindex',this.selectedNow);
        }
    }
}
</script>
<style type="text/css">
/*css中需加[v-bloak]:{display: none;}告诉vue 有v-bloak这个属性的标签暂时隐藏，vue渲染好了之后再显示*/
[v-bloak]:{display: none;}
ul{list-style: none;padding: 0;margin: 0}
.main-logo {
    width: 600px;
    height: 140px;
    position: relative;
}

.main-logo img {
    display: block;
    margin: 0px auto;
}

.logoList-arrow {
    position: absolute;
    width: 0;
    height: 0;
    border: 8px solid;
    border-color: #000 transparent transparent transparent;
    right: 100px;
    top: 66px;
    cursor: pointer
}

.logoList {
    position: absolute;
    top: 100%;
    width: 200px;
    left: 50%;
    margin-left: -100px;
    z-index: 999999;
    border: 1px solid #d4d4d4
}

.logoList li {
    width: 100%;
    height: 80px;
    background-color: #fefefe;
    line-height: 80px;
    padding-top: 1px;
}

.logoList li img {
    width: 100%;
    margin-top: 10px;
}

.logofade-enter-active,
.logofade-leave-active {
    transition: all .5s;
}

.logofade-enter,
.logofade-leave-active {
    opacity: 0;
    transform: translateY(20px);
}
.selectback{background-color: #eee !important;cursor: pointer}
</style>
