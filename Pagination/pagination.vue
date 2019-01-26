<template>
    <div class="Pagination">
        <div class="page-bar"> 
            <ul> 
                <li><a  v-on:click="goPage(cur-1)" :class="cur<=1 ? 'page-button-disabled':''">上一页</a></li>  
                <li v-for="index in indexs"  v-bind:class="{ active: cur == index }" :key="index"> 
                    <a v-on:click="goPage(index)">{{ index<1 ? "..." : index }}</a> 
                </li> 
                <li><a  v-on:click="goPage(cur+1)" :class="cur>=all ? 'page-button-disabled':''">下一页</a></li> 
            </ul> 
        </div>
    </div>
</template>
<script>
export default {
    name:'Pagination',
    props: ['cur','all'],
    computed: {
        indexs: function () {
            var left = 1;
            var right = this.all;
            var n = this.cur;
            var ar = []
            if (this.all >= 11) {
                if (n > 5 && n < this.all - 4) {
                    left = n - 5
                    right = n + 4
                } else {
                    if (n <= 5) {
                        left = 1
                        right = 10
                    } else {
                        right = this.all
                        left = this.all - 9
                    }
                }
            }
            while (left <= right) {
                ar.push(left)
                left++
            }
            if (ar[0] > 1) {
                ar[0] = 1;
                ar[1] = -1;
            }
            if (ar[ar.length - 1] < this.all) {
                ar[ar.length - 1] = this.all;
                ar[ar.length - 2] = 0;
            }
            return ar
        }
    },
    methods: {
        //上 、下一页
        goPage: function (index) {
            if (index > this.all) return;
            this.$emit('set-current-page', index);
            this.$emit('btn-click', index)
        },
    }
}
</script>
<style scoped>
ul, li {margin: 0px;padding: 0px;}
.page-bar {-webkit-touch-callout: none;-webkit-user-select: none;-khtml-user-select: none;-moz-user-select: none;-ms-user-select: none;user-select: none;}
.page-button-disabled {color:#ddd !important;pointer-events: none;}
.page-bar li {list-style: none;display: inline-block;}
.page-bar li:first-child > a {margin-left: 0px;}
.page-bar a {border: 1px solid #ddd;text-decoration: none;position: relative;float: left;padding: 6px 12px;margin-left: -1px;line-height: 1.42857143;color: #3cb7d4;cursor: pointer;}
.page-bar a:hover {background-color: #eee;}
.page-bar .active a {color: #fff;cursor: default;background-color: #1696a9;border-color: #1696a9;}
.page-bar i {font-style: normal;color: #d44950;margin: 0px 4px;font-size: 12px;}
</style>
