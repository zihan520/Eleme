<template>
	<div class="seller-warpper" ref="sellerWrapper">
		<div class="seller-content">
			<div class="info">
				<div class="title">
					<div class="text">{{seller.name}}</div>
					<div class="star-wrapper">
						<star :size="36" :score="seller.score"></star>
						<span class="count">({{seller.ratingCount}})</span>
						<span class="sellCount">月销售{{seller.sellCount}}单</span>
					</div>
					<div class="collect" @click="collectflag=!collectflag">
						<span class="icon icon-favorite" :class="{'active':collectflag}"></span><br>
						<span class="text">{{collectflag?'已收藏':'收藏'}}</span>
					</div>
				</div>
				<div class="remark">
					<div class="block">
						<h2>起送价</h2>
						<div class="content">
							<span class="num">{{seller.minPrice}}</span>元
						</div>
					</div>
					<div class="block">
						<h2>商家配送</h2>
						<div class="content">
							<span class="num">{{seller.deliveryPrice}}</span>元
						</div>
					</div>
					<div class="block">
						<h2>平均配送时间</h2>
						<div class="content">
							<span class="num">{{seller.deliveryTime}}</span>分钟
						</div>
					</div>
				</div>
			</div>
			<div class="divider"></div>
			<div class="activities">
				<h2 class="title">公告与活动</h2>
				<p class="bulletin">{{seller.bulletin}}</p>
			</div>
			<div class="supports">
				<ul>
					<li v-for="item in seller.supports" class="support-item">
						<span class="icon" :class="iconClassMap[item.type]"></span>
						<span>{{item.description}}</span>
					</li>
				</ul>
			</div>
			<div class="divider"></div>
			<div class="seller-imgs">
				<h2 class="title">商家实景</h2>
				<div class="img-wrapper" ref="picsWrapper">
					<div ref="picList">
						<img v-for="url in seller.pics" :src="url" width="120" height="90">
					</div>
				</div>
			</div>
			<div class="divider"></div>
			<div class="seller-info">
				<h2 class="title">商家信息</h2>
				<ul>
					<li v-for="info in seller.infos" class="item">{{info}}</li>
				</ul>
			</div>
		</div>
	</div>
</template>
<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import Star from 'components/star/Star'
	export default{
		components: {
			'star': Star
		},
		data() {
			return {
				seller: [],
				collectflag: false
			}
		},
		created() {
			this._init(),
			this.iconClassMap=['decrease','discount','special','invoice','guarantee'];
		},
		methods: {
			_init() {
				axios.get('static/data.json').then((res)=>{
					this.seller = res.data.seller
					this.$nextTick(() => {
						this.scroll = new BScroll(this.$refs.sellerWrapper, {
				            click: true
				          })
					})
					this._initPicScroll()
				})
			},
			_initPicScroll() {
				if(this.picsScroll){
					return
				}
				const PIC_WIDTH = 120
				const MARGIN = 6
				let picLen =this.seller.pics.length
				this.$refs.picList.style.width = PIC_WIDTH * picLen + MARGIN * (picLen - 1) + 'px'
				this.picScroll = new BScroll(this.$refs.picsWrapper,{scrollX:true})
			}
		}
	}
</script>
<style>
	.seller-warpper{
		position: absolute;
		top: 174px;
		bottom: 0;
		left: 0;
		width: 100%;
		overflow: hidden;
	}
	.seller-warpper .seller-content .info{
		padding: 18px 0;
		margin: 0 18px;
	}
	.seller-warpper .seller-content .info .title{
		padding-bottom: 18px;
		border-bottom: 1px solid rgba(7, 17, 27, .1);
	}
	.seller-warpper .seller-content .info .text{
		color: #07111b;
		font-size: 14px;
		line-height: 14px;
	}
	.seller-warpper .seller-content .info .star-wrapper{
		font-size: 0px;
		margin-top: 8px;
	}
	.seller-warpper .seller-content .info .star-wrapper .star{
		display: inline-block;
		vertical-align: top;
	}
	.seller-warpper .seller-content .info .star-wrapper .count{
		font-size: 10px;
		color:#07111b ;
		line-height: 14px;
		padding: 0 12px 0 8px;
	}
	.seller-warpper .seller-content .info .star-wrapper .sellCount{
		font-size: 10px;
		color: #4d555d;
		line-height: 14px;
	}
	.seller-warpper .seller-content .info .collect{
		position: absolute;
		right:8px;
		top: 14px;
		width: 50px;
		text-align: center;
	}
	.seller-warpper .seller-content .info .collect .icon{
		color:#d4d6d9;
		font-size: 24px;
		line-height: 24px;
	}
	.seller-warpper .seller-content .info .collect .icon.active{
		color: rgba(240, 20, 20, 1.0);
	}
	.seller-warpper .seller-content .info .collect .text{
		font-size: 10px;
		line-height: 10px;
		color: #4d555d;
		padding-top: 8px;
	}
	.seller-warpper .seller-content .info .remark{
		display: flex;
	}
	.seller-warpper .seller-content .info .remark .block{
		flex: 1;
		margin-top: 10px;
		text-align: center;
		border-right: 1px solid rgba(7, 17, 27, .1);
	}
	.seller-warpper .seller-content .info .remark>div:last-child{
		border-right: none;
	}
	.seller-warpper .seller-content .info .remark .block h2{
		font-size: 10px;
		line-height: 10px;
		color: #93999f;
		padding-bottom: 4px;
	}
	.seller-warpper .seller-content .info .remark .block .content{
		font-size: 10px;
		color: #07111b;
		line-height: 10px;
		font-weight: 200;
	}
	.seller-warpper .seller-content .info .remark .block .content .num{
		font-size: 24px;
		line-height: 30px;
	}
	.seller-warpper .divider{
		width: 100%;
		height: 16px;
		border-top: 1px solid rgba(7, 17, 27, .1);
		border-bottom: 1px solid rgba(7, 17, 27, .1);
		background-color: #f3f5f7;
	}
	.seller-warpper .activities{
		padding: 0 18px;
		margin: 18px 0 0 0;
	}
	.seller-warpper .activities h2{
		font-size: 14px;
		line-height: 14px;
		color: #07111b;
	}
	.seller-warpper .activities P{
		padding: 8px 12px 16px 12px;
		font-size: 12px;
		font-weight: 200;
		color: #f01414;
		line-height: 24px;
	}
	.seller-warpper  .supports{
		margin: 0 18px;
		font-size: 0px;
	}
	.seller-warpper .supports  .support-item{
		font-size: 0;
		padding: 16px;
		border-bottom: 1px solid rgba(7, 17, 27, .1);
	}
	.seller-warpper .supports ul>li:first-child{
		border-top: 1px solid rgba(7, 17, 27, .1);
	}
	.seller-warpper .supports .support-item span{
		font-size: 12px;
		line-height: 12px;
		font-weight: 200;
	}
	.seller-warpper  .supports  .support-item .icon{
		display: inline-block;
		vertical-align: top;
		width: 16px;
		height: 16px;
		margin-right: 6px;
		background-size: 16px 16px !important;
		background-repeat: no-repeat;
	}
	.seller-warpper  .supports  .support-item .decrease{
		background: url('decrease_3@2x.png');
	}
	.seller-warpper  .supports  .support-item .discount{
		background: url('discount_3@2x.png');
	}
	.seller-warpper  .supports  .support-item .guarantee{
		background: url('guarantee_3@2x.png');
	}
	.seller-warpper  .supports  .support-item .invoice{
		background: url('invoice_3@2x.png');
	}
	.seller-warpper  .supports  .support-item .special{
		background: url('special_3@2x.png');
	}
	.seller-warpper  .seller-imgs{
		margin:18px;
		white-space: nowrap;
		overflow: hidden;
	}
	.seller-warpper  .seller-imgs, .title{
		font-size: 14px;
		line-height: 14px;
		color: #07111b;
		margin-bottom: 12px;
	}
	.seller-warpper  .seller-imgs img{
		margin-right: 6px;
	}
	.seller-warpper .seller-info{
		margin: 0px 18px;
	}
	.seller-warpper .seller-info .title{
		padding: 18px 0 12px 0;
		font-size: 16px;
		line-height: 16px;
		color: #07111b;
	}
	.seller-warpper .seller-info .item{
		padding: 16px 12px;
		font-size: 12px;
		line-height: 16px;
		border-top: 1px solid rgba(7, 17, 27, .1);
		color: #07111b;
	}
</style>