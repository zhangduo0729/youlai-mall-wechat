<template>
    <div>
        <div v-if="!isShowLoading">
            <!-- 轮播图 -->
            <ad-carousel :advertList="advertList"/>

            <!-- 分类导航 -->
            <category-nav :categoryList="categoryList"/>

            <!-- 秒杀专区 -->
            <spike :seckillGoodsList="seckillGoodsList"/>

            <hot :hotGoodsList="hotGoodsList"/>
            <!-- 回到顶部按钮 -->
            <v-top/>
        </div>
        <!-- 数据加载提示gif -->
        <loading :show="isShowLoading"/>
    </div>
</template>

<script>
    import {advertList} from '@/api/home'
    import {categoryList} from '@/api/category'
    import {goodsList} from '@/api/goods'


    import loading from '@/components/loading/LoadingGif'
    import AdCarousel from './components/AdCarousel'
    import CategoryNav from './components/CategoryNav'
    import Spike from './components/Seckill'
    import Hot from './components/Hot'

    export default {
        created() {
            this._initData()
        },
        data() {
            return {
                isShowLoading: false,     // 是否加载动画
                advertList: [],   // 广告轮播图
                categoryList: [],   // 商品分类
                seckillGoodsList: {},        // 秒杀商品
                hotGoodsList: []
            }
        },
        components: {
            loading,
            AdCarousel,
            CategoryNav,
            Spike,
            Hot
        },
        methods: {
            _initData() {

                // 广告轮播
                let params = {
                    limit: 5   // 广告条数
                }

                advertList(params).then(response => {
                    this.advertList = response.data
                })


                params = {
                    limit: 10, // 分类条数
                    level: 1 //分类级别
                }

                categoryList(params).then(response => {
                    this.categoryList = response.data
                })

                goodsList().then(response=>{
                    this.hotGoodsList=response.data
                })


                goodsList().then(response => {
                    this.seckillGoodsList = response.data
                })

                this.isShowLoading = false
            }
        }
    }
</script>

<style lang="less" scoped>

</style>