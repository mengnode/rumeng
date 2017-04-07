<template>
	<div class="MovieDetail">
		<div class="header">
			<span class="back"><</span>
			<h1>{{MovieDetail.title}}</h1>
		</div>
		<div class="info">
			<img class="cover" :src="MovieDetail.images.medium">
			<p class="t">{{MovieDetail.title}}</p>
			<star v-bind:num="MovieDetail.rating.stars"></star>
			<p >{{MovieDetail.rating.average}}({{MovieDetail.collect_count}}人评论)</p>
			<p>{{MovieDetail.year}}年</p>
			<p>{{MovieDetail.genres.join(",")}}</p>
			<p>{{MovieDetail.countries.join(" ")}}</p>
			<p>{{MovieDetail.pubdate}}</p>
		</div>
		<div class="main">
			<div class="want">
				<div class="item">{{MovieDetail.wish_count}}人想看</div>
				<div class="item">{{MovieDetail.reviews_count}}人看过</div>
			</div>
			<p class="summary">
			{{MovieDetail.summary}}
			</p>
			<h6>演职人员</h6>
			<ul class="casts">
				<li v-for="cast in MovieDetail.casts">
					<img :src="cast.avatars.medium">
					<p>{{cast.name}}</p>
				</li>
			</ul>
			<h6>热门短评</h6>
			<div class="comments">
				<div class="item" v-for="c in MovieDetail.popular_comments">
					<p class="t"><star class="star" v-bind:num="c.rating.value*10"></star><span>{{c.created_at}}</span></p>
					<p class="c">{{c.content}}</p>
					<p class="a"><img :src="c.author.avatar"><span>{{c.author.name}}</span></p>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
	import star from "./star";
	export default{
		components:{
			star
		},
		data(){
			return {
				MovieDetail:{
					title:"",
					images:{
						medium:""
					},
					rating:{
						stars:0
					},
					genres:[],
					countries:[]
				}
			}
		},
		mounted:function(){
			var id = this.$route.params.id;
			this.$http.jsonp("https://api.douban.com/v2/movie/subject/"+id+"?apikey=0b2bdeda43b5688921839c8ecb20399b").then(function(response){
		  		this.MovieDetail = response.body;
		  	})
		}
	}
</script>
<style scoped>
	.header{
		height:50px;
		background: #e54847;
		color:#fff;
		line-height: 50px;
	}
	.header .back{
		float: left;
		text-align: center;
		width: 50px;
		font-size:24px;
	}
	.header h1{
		padding-right: 50px;
		text-align: center;
		font-size:20px;
	}
	.info{
		background: #b4b1b1;
		color: #6b6868;
		overflow: hidden;
		padding:10px 20px;
	}
	.info .cover{
		float: left;
		border:1px solid #fff;
		margin-right:20px;
	}
	.info .t{
		font-size: 18px;
		line-height: 1em;
		margin-bottom: 10px;
		font-weight: 500;
	}
	.info p{
		font-size: 12px;
		line-height: 20px;
	}
	.main{
		background: #e5e9f2;
		padding:20px;
	}
	.main .want{
		text-align: center;
	}
	.main .want .item{
		margin:0px 10px;
		width:120px;
		height: 30px;
		line-height: 30px;
		text-align: center;
		font-size:14px;
		border-radius: 5px;
		color:#fff;
		background:#e54847;
		display: inline-block;
	}
	.main .summary{
		margin-top:10px;
		font-size: 14px;
		line-height: 1.5em;
	}
	.main h6{
		font-size:15px;
		font-weight: 700;
		color:#666;
		margin:10px 0px;
	}
	.main .casts{
		overflow-x:scroll;
		overflow-y: hidden;
	    white-space: nowrap;
	}
	.main .casts li{
		display: inline-block;
		margin-right:20px;
	}
	.main .casts li p{
	    width: 70px;
	    white-space: nowrap;
	    overflow: hidden;
	    text-overflow: ellipsis;
	    font-size: 12px;
	}
	.main .casts li img{
		width: 70px;
	}			
	.main .comments .item{
		border-bottom: 1px solid #ccc;
		padding-bottom: 10px;
	}

	.main .comments .item .star{
		display: inline-block;
	}
	.main .comments .item .t *{
		vertical-align: bottom;
	}
 	.main .comments .item .t span{
		font-size: 12px;
		line-height: 21px;
	}
	.main .comments .item .c{
		line-height: 1.5em;
		font-size:12px;
		margin:5px 0px;
	}
	.main .comments .item .a *{
		vertical-align: middle;
		font-size:14px;
	}
	.main .comments .item .a img{
		width:24px;
		border-radius: 12px;
	}


</style>
