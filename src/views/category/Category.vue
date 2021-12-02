<template>
	<div class="wrapper" ref="aaaa">
		<ul class="content">
			<button @click="btnClick">按钮</button>
			<li>列表1</li>	
			<li>列表2</li>	
			<li>列表3</li>	
			<li>列表4</li>	
			<li>列表5</li>	
			<li>列表6</li>	
			<li>列表7</li>	
			<li>列表8</li>	
			<li>列表9</li>	
			<li>列表10</li>	
			<li>列表11</li>	
			<li>列表12</li>	
			<li>列表13</li>	
			<li>列表14</li>	
			<li>列表15</li>	
			<li>列表16</li>	
			<li>列表17</li>	
			<li>列表18</li>	
			<li>列表19</li>	
			<li>列表1</li>
			<li>列表2</li>	
			<li>列表3</li>	
			<li>列表4</li>	
			<li>列表5</li>	
			<li>列表6</li>	
			<li>列表7</li>	
			<li>列表8</li>	
			<li>列表9</li>	
			<li>列表10</li>	
			<li>列表11</li>	
			<li>列表12</li>	
			<li>列表13</li>	
			<li>列表14</li>	
			<li>列表15</li>	
			<li>列表16</li>	
			<li>列表17</li>	
			<li>列表18</li>	
			<li>列表19</li>	
		</ul>
	</div>
</template>

<script>
	import BScroll from 'better-scroll'
	
	export default{
		name:"Category",
		data(){
			return{
				scroll:null
			}
		},
		created(){
			// this.scroll = new BScroll('.wrapper',{
				
			// })
		},
		mounted(){
			this.scroll = new BScroll(document.querySelector('.wrapper'),{
				probeType:3,
				pullUpLoad:true
			})
			this.scroll.on('scroll',(position) =>{
				// console.log(position);
			})
			this.scroll.on('pullingUp',() =>{
				console.log('上拉加载更多');
			})
		},
		methods:{
			btnClick(){
				console.log('btnClick');
			}
		}
	}
</script>

<style scoped>
	.wrapper{
		height:150px;
		background-color:red;
		overFlow:hidden;
		/* overFlow-y:scroll; */
	}
</style>
