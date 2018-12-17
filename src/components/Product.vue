<template>
    <div class="product product_horizontal">
        <div class="product_content_block">
            <div class="product_photo">
                <a href="#" class="url--link product__link">
                    <img class="product_image" v-bind:src="item.primaryImageUrl | cropped">
                </a>
            </div>
            <div>
                <div class="product_attributes">
                    <span class="product_code">Код: {{item.code}}</span>
                    <span v-if="item.isActive = true" class="product_status">Наличие</span>
                </div>
                <h3 class="product_description">
                    <a href="#" class="product__link">{{item.title}}</a>
                </h3>
                <div class="product_tags hidden-sm">
                    <p><b>Могут понадобиться:</b>
                        <a href="#" v-for="(assocItem, assocIndex) in item.assocProducts"
                           :key="index + assocIndex + assocItem" class="url--link">
                            {{assocItem }}
                        </a>
                    </p>
                </div>
            </div>
        </div>
        <div>
            <div class="product_price_block">
                <span class="product_price_club_card_text">По карте<br>клуба</span>
                <div>
                    <div class="product_price_club_card">
                            <span v-if="showPrice" class="goldPrice">
                                {{item.priceGoldAlt.toFixed(2)}} &#8381;
                            </span>
                        <span v-if="!showPrice" class="goldPrice">
                                {{item.priceGold.toFixed(2)}} &#8381;
                            </span>
                        <span class="rouble__i black__i"></span>
                    </div>
                    <div class="product_price_default">
                            <span v-if="showPrice" class="retailPrice">
                                {{item.priceRetailAlt.toFixed(2)}} &#8381;
                            </span>
                        <span v-if="!showPrice" class="retailPrice">
                                {{item.priceRetail.toFixed(2)}} &#8381;
                            </span>
                        <span class="rouble__i black__i"></span>
                    </div>
                </div>
            </div>
            <div class="list--unit-desc">
                <div class="product_units">
                    <div class="unit--wrapper">
                        <div v-on:click="showPrice=true" class="unit--select" v-bind:class="{ 'unit--active': showPrice }">
                            <p class="ng-binding">За м. кв.</p>
                        </div>
                        <div v-on:click='showPrice=false' class="unit--select" v-bind:class="{ 'unit--active': !showPrice }">
                            <p class="ng-binding">За упаковку</p>
                        </div>
                    </div>
                </div>
                <div class="unit--info">
                    <div class="unit--desc-t">
                        <span class="ng-binding">Единица измерения: {{item.unitFull}}</span><br>
                        <span class="unit--infoInn">{{goodNumber}} {{item.unit}} = {{(item.unitRatioAlt.toFixed(1)*goodNumber).toFixed(1)}} {{item.unitAlt}}</span>
                    </div>
                </div>
            </div>
            <div class="product__wrapper">
                <div class="product_count_wrapper">
                    <div class="stepper">
                        <button class="button-click __lest" v-on:click="prevCount">
                            <svg width="7" height="10" viewBox="0 0 7 10" fill="none"
                                 xmlns="http://www.w3.org/2000/svg">
                                <path d="M6.18333 1.175L2.35833 5L6.18333 8.825L5 10L0 5L5 0L6.18333 1.175Z"
                                      fill="#4D4D4D"/>
                            </svg>
                        </button>
                        <button class="button-click __count">{{count}}</button>
                        <button class="button-click __right" v-on:click="nextCount">
                            <svg width="7" height="10" viewBox="0 0 7 10" fill="none"
                                 xmlns="http://www.w3.org/2000/svg">
                                <path d="M0.816667 1.175L4.64167 5L0.816667 8.825L2 10L7 5L2 0L0.816667 1.175Z"
                                      fill="#4D4D4D"/>
                            </svg>
                        </button>
                    </div>
                <button class="btn btn_cart" data-url="/cart/"
                        v-bind:data-product-id="item.productId">
                    <span class="ng-binding">В корзину</span>
                </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Product",
        props:['item', 'index'],
        data() {
            return {
                count: 1,
                goodNumber: 1,
                showPrice: true
            };
        },

        methods: {
            prevCount() {
                if (this.count > 1) {
                    this.count -= 1;
                    this.goodNumber -= 1;
                }
            },
            nextCount() {
                this.count += 1;
                this.goodNumber += 1;
            },
        },
        filters: {
            cropped: function (url) {
                const imageSize = '_220x220_1';
                let urlImg = url.slice(0, -4);
                let urlExt = url.slice(url.length-4);
                return `${urlImg}${imageSize}${urlExt}`;
            }
        }
    }
</script>

<style scoped lang="sass">
    *
        box-sizing: border-box
        margin: 0
        padding: 0

        a
            text-decoration: none
            color: black

        button
            cursor: pointer

        .product_horizontal
            border: 1px saddlebrown solid
            display: flex

            .product_content_block
                display: flex
                width: 80%

                .product_photo
                    padding: 10px

                .product_attributes
                    display: flex
                    padding: 10px
                    span
                        margin-right: 15px
                    .product_code
                        color: gray
                    .product_status
                        color: green
                        text-decoration: underline

                .product_description
                    font-size: 20px
                    margin: 10px

                .product_tags
                    font-size: 15px
                    margin: 10px

            .product_price
                &_block
                    display: flex
                    margin-top: 10px
                &_club_card_text
                    font-size: 12px
                    font-weight: bold
                &_club_card
                    margin-left: 5px
                    font-size: 20px
                    font-weight: bold
                &_default
                    margin-left: 5px
                    font-size: 20px

            .unit--wrapper
                display: flex
                justify-content: space-around
                .unit--select
                    text-decoration: underline
                    text-decoration-style: dotted
                    cursor: pointer
                .unit--active
                    background-color: black
                    color: white
                    padding: 2px
                    text-decoration: none

            .unit--desc-t
                font-size: 12px
                border: 0.5px solid black
                margin-top: 10px
                margin-bottom: 20px
                padding: 7px
                position: relative
            .unit--desc-t::before
                content: " "
                position: absolute
                background: #ffffff
                width: 12px
                height: 12px
                border-style: solid
                border-color: black
                border-width: 0.5px 0 0 0.5px
                -webkit-transform: rotate(225deg)
                left: 10px
                bottom: -7px

        .product_count_wrapper
            display: flex
            .stepper
                margin-right: 10px
                .button-click
                    width: 25px
                    height: 25px

            .btn_cart
                padding: 5px
                background-color: orange
                color: white
                text-transform: uppercase
                font-size: 10px
</style>