<template>
    <div>
        <nav-header></nav-header>
        <nav-bread>
            <span slot="1">h11aha</span>
            <span slot="2">hah222a</span>
        </nav-bread>
        <div class="accessory-result-page accessory-page">
            <div class="container">
                <div class="filter-nav">
                    <span class="sortby">Sort by:</span>
                    <a href="javascript:void(0)" class="default cur">Default</a>
                    <a href="javascript:void(0)" class="price">Price
                        <svg class="icon icon-arrow-short">
                            <use xlink:href="#icon-arrow-short"></use>
                        </svg>
                    </a>
                    <a href="javascript:void(0)" class="filterby stopPop" @click="priceShow()">Filter by</a>
                </div>
                <div class="accessory-result">
                    <!-- filter -->
                    <div class="filter stopPop" id="filter" v-bind:class="{'filterby-show':isPriceShow}">
                        <dl class="filter-price">
                            <dt>Price:</dt>
                            <dd>
                                <a href="javascript:void(0)" v-bind:class="{'cur':priceChecked == 'all'}" @click="priceChecked ='all'">All</a>
                            </dd>
                            <dd v-for="(price,index) in priceFilter">
                                <a href="javascript:void(0)"  
                                    @click="postIdx(index)" 
                                    v-bind:class="{
                                        'cur':priceChecked==index
                                    }"
                                >{{price.startPrice}} - {{price.endPrice}}</a>
                            </dd>
                        </dl>
                    </div>

                    <!-- search result accessories list -->
                    <div class="accessory-list-wrap">
                        <div class="accessory-list col-4">
                            <ul>
                                <li v-for="item in goodList">
                                    <div class="pic">
                                        <a href="#"><img v-lazy="'/static/'+item.prodcutImg" alt=""></a>
                                    </div>
                                    <div class="main">
                                        <div class="name">{{item.productName}}</div>
                                        <div class="price">{{item.prodcutPrice}}</div>
                                        <div class="btn-area">
                                            <a href="javascript:;" class="btn btn--m">加入购物车</a>
                                        </div>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="md-overlay" v-show="wrapShow" @click="closeWrap()"></div>
        <nav-footer></nav-footer>
    </div>
</template>
<script>
    import "./../assets/css/base.css"
    import "./../assets/css/product.css"
    import "./../assets/css/checkout.css"
    import "./../assets/css/login.css"

    import NavHeader from '@/components/Header'
    import NavFooter from '@/components/Footer'
    import NavBread from '@/components/Bread'
    // import goods from '../../mock/goods.json'
    import axios from 'axios'
    export default {
        data() {
            return {
                goodList: [],
                priceFilter: [
                    {
                        startPrice: "100",
                        endPrice: "500"
                    },
                    {
                        startPrice: "500",
                        endPrice: "1000"
                    },
                    {
                        startPrice: "1000",
                        endPrice: "1500"
                    }
                ],
                priceChecked:"all",
                isPriceShow:false,
                wrapShow:false
            }
        },
        components: {
            NavHeader,
            NavFooter,
            NavBread,
            // goods
        },
        mounted: function () {
            this.getGoodList()
        },
        methods: {
            getGoodList() {
                axios.get('/api/result').then((req) => {
                    console.log(req)
                    let res = req.data;
                    this.goodList = res;
                }).catch((res) => {
                    console.log(res)
                })
            },
            postIdx(idx){
                this.priceChecked = idx;
                this.closeWrap();
            },
            priceShow(){
                this.isPriceShow = !this.isPriceShow
                this.wrapShow = !this.wrapShow
            },
            closeWrap(){
                this.isPriceShow = !this.isPriceShow
                this.wrapShow = !this.wrapShow
            }
        }
    };
</script>
