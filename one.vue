<template>
 <div>
  <div>
	<input type="text" v-model="value" placeholder="请输入学生姓名" >
	<p>
		<span @click="handUp(tableData)">升序</span><span @click="handDown(tableData)">降序</span>
	</p>
  </div>

	<table>
		<thead>
			<tr>
				<th v-for="item in headList" :key="item">{{item}}</th>
				<th v-for="(sub,i) in subjectList" :key="i" class="flexTh">{{sub.name}}</th>
				<th>操作</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="(item,i) in newData" :key="i">
				<td >{{i+1}}</td>
				<td >{{item.name}}</td>
				<td >{{item.sex}}</td>
				<td >{{item.age}}</td>
				<td >{{item.fullscore}}</td>
				<td>{{item.chinese}}</td>
				<td>{{item.math}}</td>
				<td>{{item.english}}</td>
				<td class="up" @click="handupIndex(i)"></td>
			</tr>
			<tr v-show="newData.length==0">
				<td :colspan='headList.length+subjectList.length+1'>{{meg}}</td>
			</tr>
		</tbody>
	</table>
	<div>
		<p>总计{{tableData.length}}条，每页显示6条，当前第1页</p>
	</div>
	<Pagination v-bind:cur="cur" v-bind:all="all" v-on:btn-click="listen" v-on:set-current-page="setCurrentPage" ></Pagination>


 </div>
</template>

<script>
import Pagination from '../Pagination/pagination'
export default {
	  components: {Pagination,},
	  name: 'Sale',
      data() {
        return {
			cur: 1, //当前页
        	all:10,  //所有页面
			headList:['序号','姓名','性别','年龄','总分'],
		  	tableData:[
				  {'num':1,'name':'lilo','sex':'女','age':18,'fullscore':33,'chinese':22,'math':10,'english':1},
				  {'num':2,'name':'abc','sex':'男','age':22,'fullscore':13,'chinese':3,'math':5,'english':5},
				  {'num':3,'name':'hohoy','sex':'女','age':26,'fullscore':83,'chinese':33,'math':30,'english':20},
				  {'num':4,'name':'hoho','sex':'女','age':26,'fullscore':83,'chinese':33,'math':30,'english':20},
				  {'num':5,'name':'xixi','sex':'女1','age':25,'fullscore':99,'chinese':44,'math':45,'english':0},
				  {'num':6,'name':'pp','sex':'女2','age':21,'fullscore':83,'chinese':33,'math':30,'english':20},],
			subjectList:[{name:'语文'},{name:'数学'},{name:'英语'}],
			value:'',
			meg:'',
        }
      },
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         


	  mounted() {
			this.all = this.tableData.length/6;
	  },
	  computed:{
		 newData: function(){
			var that = this;
			var data = that.tableData.slice();
			var filterKey = that.value && that.value.toLowerCase();
			if( filterKey ){
				 data = data.filter(function(row) {
						return Object.keys(row).some(function(key) {
								return String(row[key]).toLowerCase().indexOf(filterKey) > -1
						})
					})
			}
			if( data.length==0){
				that.meg='暂无相关信息';
				return data;
			}else{
				return data;
			}
		 },
		 studentList:function(){
			var that = this;
			var data = that.stulist;
			var std_name = that.studentName && that.studentName.toLowerCase();
			if( std_name ){
				data = data.filter(function(val){
					return Object.keys(val).some(function(key){
						return String(val[key]).toLowerCase().indexOf(std_name) > -1
					})
				})
			}
			return data
		 }
	  },
      methods: {
		handupIndex(index){
			//Vue.set() 响应式新增与修改数据 
			if( index != 0){
				let temp = this.tableData[index-1];
				this.$set(this.tableData, index-1, this.tableData[index])
				this.$set(this.tableData, index, temp)
			}
		},
		//升序  降序
		handUp(data){
			data.sort((a,b)=>{
                return a.score-b.score;
			});
			this.tableData = data;
		},
		handDown(data){
			data.sort((a,b)=>{
                return b.score-a.score;
			});
			this.tableData = data;
		},
		listen: function (data) {
			console.log('当前页码：' + data )
		},
		 // 设置当前页码
        setCurrentPage: function(index){
			this.cur = index;
        },
	},
}
</script>
 
<style>
.li{list-style-type:none;}
table, tr,th,td{border: 1px solid #f2f2f5;padding: 10px;}
table{border-collapse:collapse;}
td,th{text-align: center;}
p span,.up{width: 50px;height: 25px;text-align: center;cursor: pointer;margin: 0 5px;}
p span:nth-child(1)::after,.up::after{content: '\2191';}
p span:nth-child(2)::after{content: '\2193';}
p{cursor: pointer;}
.notMsg{width: 364px;text-align: center;padding: 10px 0; border: 2px solid #3cb7d4; border-top: none;}


table {border: 2px solid #3cb7d4; margin: 10px;}
        th {background: #3cb7d4;}
        td {padding: 10px 20px;min-width: 60px; background: rgb(231, 226, 226);text-align: center;}
        input {margin-bottom: 7px;border: 1px solid #42b983; height: 2em;line-height: 2em;display: inline-block; padding: 0 3px;}
        .arrow {margin-left: 3px;vertical-align: middle;width: 0;height: 0;cursor: pointer;}
        .arrow.dsc { display: inline-block; border: 4px solid #ccc;border-bottom: 0; border-color: #ccc transparent transparent;}
        .arrow.asc {display: inline-block;border: 4px solid #ccc; border-top: 0; border-color: transparent transparent #ccc;
	}
.ul{width: 350px;}
.ul li{width: 80px;height: 50px;background-color: sandybrown;border: 1px solid #f2f2f5;float: left;list-style: none;text-align: center;line-height: 0;}
.ul li p{padding: 0;}
</style>
