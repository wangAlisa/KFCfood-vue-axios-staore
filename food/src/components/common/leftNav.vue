<template>
	<div class="left-nav">
		<ul>
			<li>
				<i class = "icon iconfont icon-jinrudianpu"></i>
				<div>收银</div>
			</li>
			<li>
				<i class = "icon iconfont icon-gongnengjianyi"></i>
				<div>店铺</div>
			</li>
			<li>
				<i class = "icon iconfont icon-guanzhudianpu"></i>
				<div>店铺</div>
			</li>
			<li>
				<i class = "icon iconfont icon-gouwucheman"></i>
				<div>店铺</div>
			</li>
			<li>
				<i class = "icon iconfont icon-kefuyouxian"></i>
				<div>设置</div>
			</li>
		</ul>
	</div>
</template>
<script>
	export default {
		name : "leftNav"
	}
</script>
<style>
	.left-nav{
		color:#fff;
       font-size:10px;
       height:100%;
       background-color: #1D8ce0;
       float:left;
       width:5%;
	}
	.iconfont{
		font-size: 24px;
	}
	.left-nav ul {
		padding: 0px;
		margin:0px;
	}
	.left-nav li {
		list-style: none;
		border-bottom: 1px solid #20a0ff;
		text-align: center;
		padding:10px;
	}
</style>