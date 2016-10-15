<template>
    <div>
        <!-- vue2.0中组件不能写到template的根节点上，即每个组件只有一个根节点，所以要把logo-select写到div中，div就是这个template的根节点 -->

       <!--  getindex是自定义事件,html中对驼峰式写法有严格的要求，所以最好不要写成getIndex，可以写成get-index, -->
        <logo-select  @getindex='getIndex'></logo-select>
        <div class="search-input">
            <!-- $event是实参，表示event对象 -->
            <input type="text" v-model="keyword" @keyup="get($event)" @keydown.enter="search()" @keydown.down="selectDown()" @keydown.up.prevent="selectUp()">
            <span class="search-reset" @click="clearInput()">&times;</span>
            <button class="search-btn" @click="search()">搜一下</button>
            <div class="search-select">
            <!-- transition-group也是vue2.0中的新特性,tag='ul'表示用ul包裹v-for出来的li，可以看2.0文档中的过渡一章 -->
                <transition-group name="itemfade" tag="ul" mode="out-in" v-cloak>
                    <li v-for="(value,index) in myData" :class="{selectback:index==now}" class="search-select-option search-select-list" @mouseover="selectHover(index)" @click="selectClick(index)" :key="value">
                        {{value}}
                    </li>
                </transition-group>
            </div>
        </div>
    </div>
</template>

<script type="text/javascript">
// import xxx from someSrc     es6中得到模块的方法。
import logoSelect from './logo-select.vue';

export default {
    // 注册组件，如果上面的logoSelect变成logoSelect1，components对象中也要变成logoSelect1，<template>标签中要变成<logo-select1>
    components: {
        logoSelect
    },
    data: function() {
        return {
            myData: [],
            keyword: '',
            now: -1,
            searchIndex: 0,
            logoData: [{
                name: '360搜索',
                searchSrc: 'https://www.so.com/s?ie=utf-8&shb=1&src=360sou_newhome&q='
            }, {
                name: '百度搜索',
                searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd='
            }, {
                name: '搜狗搜索',
                searchSrc: 'https://www.sogou.com/web?query='
            }]
        }
    },
    methods: {
        // &event是实参，表示event对象
        get: function(ev) {
            if (ev.keyCode == 38 || ev.keyCode == 40) {
                return;
            }
            this.$http.jsonp('https://sug.so.360.cn/suggest?word=' + this.keyword + '&encodein=utf-8&encodeout=utf-8').then(function(res) {
                //                        console.log(res.data.s);
                this.myData = res.data.s;

            }, function() {

            });
        },
        selectDown: function() {
            this.now++;
            if (this.now == this.myData.length) {
                this.now = 0;
            }
            this.keyword = this.myData[this.now];
        },
        selectUp: function() {
            this.now--;
            if (this.now == -1) {
                this.now = this.myData.length - 1;
            }
            this.keyword = this.myData[this.now];
        },
        search: function() {
            window.open(this.logoData[this.searchIndex].searchSrc + this.keyword);
        },
        selectHover: function(index) {
            this.now = index
        },
        selectClick: function(index) {
            this.keyword = this.myData[index];
            this.search();
        },
        clearInput: function() {
            this.keyword = '';
            this.$http.jsonp('https://sug.so.360.cn/suggest?word=' + this.keyword + '&encodein=utf-8&encodeout=utf-8').then(function(res) {
                //                        console.log(res.data.s);
                this.myData = res.data.s;

            }, function() {

            });
        },
        getIndex: function(index) {
            this.searchIndex = index;
        }
    }
}
</script>
<style type="text/css">
.search-input {
    height: 45px;
    width: 600px;
    margin: 0px auto;
    margin-top: 10px;
    position: relative;
}

.search-input input {
    border: 1px solid #e4e4e4;
    box-sizing: border-box;
    width: 500px;
    height: 45px;
    font-size: 18px;
    float: left;
    padding-left: 10px;
    padding-right: 10px;
    overflow: hidden;
}

.search-btn {
    height: 45px;
    width: 100px;
    border: 1px solid mediumseagreen;
    background-color: mediumseagreen;
    color: white;
    font-size: 16px;
    font-weight: bold;
    float: left;
}

.search-btn {
    cursor: pointer
}

.search-select {
    position: absolute;
    top: 45px;
    width: 500px;
    box-sizing: border-box;
    z-index: 999;
}

.search-select li {
    border: 1px solid #d4d4d4;
    ;
    border-top: none;
    border-bottom: none;
    background-color: #fff;
    width: 100%
}

.search-select-option {
    box-sizing: border-box;
    padding: 7px 10px;
}

.selectback {
    background-color: #eee !important;
    cursor: pointer
}

input::-ms-clear {
    display: none
}

.search-reset {
    width: 21px;
    height: 21px;
    position: absolute;
    display: block;
    line-height: 21px;
    text-align: center;
    cursor: pointer;
    font-size: 20px;
    right: 110px;
    top: 12px
}

.search-select-list {
    transition: all 0.5s
}

.itemfade-enter,
.itemfade-leave-active {
    opacity: 0;
}

.itemfade-leave-active {
    position: absolute;
}

[v-cloak] {
    display: none;
}

.selectback {
    background-color: #eee !important;
    cursor: pointer
}
.search-select ul{margin:0;text-align: left; }
</style>
