<template>
    <div class="container mx-auto">
        <section class="container mx-auto" id="favourite-game">
        <div class="text-center">
            <h2 class="text-4xl text-red-300 font-bold">Shop Products</h2>
            <div class="grid grid-cols-1 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-5 gap-2 md:gap-4 p-2 lg:p-0">
            <div
                v-for="product in products"
                :key="product.id"
                class="rounded overflow-hidden shadow-lg shadow-manual border-2 mt-5 bg-white"
            >
                <router-link
                :to="{
                    name: 'shop-details',
                    params: { id: product.id, slug: makeSlug(product.name) }
                }"
                >
                <!-- params: { id: product.id,slug: makeSlug(product.name) }, -->
                <div id="outer-div">
                    <img
                        class="w-full h-48 pt-6 pl-6 pr-6 manual-img"
                        :src="'/product/' + product.logo"
                        v-bind:alt="product.name"
                    />
                </div>
                <div class="px-2 py-2">
                    <div class="font-medium text-base mb-1">{{ product.name }}</div>
                    <div class="font-medium text-base mb-2" style="display: inline-flex;"><p class="text-2xl font-medium mr-1 font-black" style="margin-top: -8px;">&#2547;</p> <div>{{ product.sale_price }}</div></div>
                </div>
                </router-link>
                <div v-if="checkifatcart(product.id)" class="grid grid-cols-3 gap-4 justify-center items-center mx-3">
                    <div class="justify-center items-center text-center">
                        <p class="text-white bg-orange-500 text-white font-normal py-2 px-5 rounded btn" style="border-radius: 28px;">
                            {{ cartArray(product.id).quantity }}
                        </p>
                    </div>
                    <div class="justify-center items-center text-center">
                        <button v-if="cartArray(product.id).quantity > 1" class="rounded-full border-2 border-orange-500 p-2 hover:bg-orange-500 fucos:outline-none" @click="minusQuntity(product)">
                            <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="minus" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="svg-inline--fa fa-minus fa-w-14 w-6 h-6 text-orange-500"><path fill="currentColor" d="M416 208H32c-17.67 0-32 14.33-32 32v32c0 17.67 14.33 32 32 32h384c17.67 0 32-14.33 32-32v-32c0-17.67-14.33-32-32-32z" class=""></path></svg>
                        </button>
                        <button v-else class="rounded-full border-2 border-pink-500 p-2 hover:bg-pink-500 fucos:outline-none" @click.prevent="removeFromCart(product)">
                            <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="minus" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="svg-inline--fa fa-minus fa-w-14 w-6 h-6 text-pink-500"><path fill="currentColor" d="M416 208H32c-17.67 0-32 14.33-32 32v32c0 17.67 14.33 32 32 32h384c17.67 0 32-14.33 32-32v-32c0-17.67-14.33-32-32-32z" class=""></path></svg>
                        </button>
                    </div>
                    <div class="justify-center items-center text-center">
                        <button class="rounded-full border-2 border-orange-500 p-2 hover:bg-orange-500 fucos:outline-none" @click="plusQuntity(product)">
                            <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="plus" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="svg-inline--fa fa-plus fa-w-14 w-6 h-6 text-orange-500"><path fill="currentColor" d="M416 208H272V64c0-17.67-14.33-32-32-32h-32c-17.67 0-32 14.33-32 32v144H32c-17.67 0-32 14.33-32 32v32c0 17.67 14.33 32 32 32h144v144c0 17.67 14.33 32 32 32h32c17.67 0 32-14.33 32-32V304h144c17.67 0 32-14.33 32-32v-32c0-17.67-14.33-32-32-32z" class=""></path></svg>
                        </button>
                    </div>
                </div>
                <p v-else @click="addToCart(product)" class="flex justify-center mx-3 mb-3 items-center text-white cursor-pointer bg-orange-500 hover:bg-red-300 text-white font-normal py-2 px-2 rounded">
                    <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="cart-plus" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512" class="svg-inline--fa fa-cart-plus fa-w-18 h-5 w-5 mr-1"><path fill="currentColor" d="M504.717 320H211.572l6.545 32h268.418c15.401 0 26.816 14.301 23.403 29.319l-5.517 24.276C523.112 414.668 536 433.828 536 456c0 31.202-25.519 56.444-56.824 55.994-29.823-.429-54.35-24.631-55.155-54.447-.44-16.287 6.085-31.049 16.803-41.548H231.176C241.553 426.165 248 440.326 248 456c0 31.813-26.528 57.431-58.67 55.938-28.54-1.325-51.751-24.385-53.251-52.917-1.158-22.034 10.436-41.455 28.051-51.586L93.883 64H24C10.745 64 0 53.255 0 40V24C0 10.745 10.745 0 24 0h102.529c11.401 0 21.228 8.021 23.513 19.19L159.208 64H551.99c15.401 0 26.816 14.301 23.403 29.319l-47.273 208C525.637 312.246 515.923 320 504.717 320zM408 168h-48v-40c0-8.837-7.163-16-16-16h-16c-8.837 0-16 7.163-16 16v40h-48c-8.837 0-16 7.163-16 16v16c0 8.837 7.163 16 16 16h48v40c0 8.837 7.163 16 16 16h16c8.837 0 16-7.163 16-16v-40h48c8.837 0 16-7.163 16-16v-16c0-8.837-7.163-16-16-16z" class=""></path></svg>    
                    Add To Cart 
                </p>
            </div>
            </div>
        </div>
        </section>
    </div>
</template>

<script>
import axios from "axios";
import { mapGetters } from "vuex";
export default {
    data: () => ({
        products: [],
    }),
    computed:{
        ...mapGetters({
            cart: "cart/cart",
            cartCount: "cart/cartCount"
        }),
    },
    methods: {
        removeFromCart(item) {
			this.$store.dispatch('cart/removeFromCart', item)
		},
        plusQuntity(item) {
            this.$store.dispatch("cart/plusQuntity", item);
        },
        minusQuntity(item) {
            var value =  this.cartArray(item.id);
            if (value.quantity > 1) {
                this.$store.dispatch("cart/minusQuntity", item);
            }
        },
        cartArray(id){
            return this.cart.find(item => item.id === id);
        },
        checkifatcart(id){
            let istrue=false;
            this.cart.forEach((element) => {
                if(id==element.id){
                   istrue=true; 
                }
            })
            return istrue;
        },
        loadProducts() {
            axios.get("/api/shop-products").then(response => {
                this.products = response.data;
            });
        },
        makeSlug(slug) {
            var words = slug.split(" ");
            for (var i = 0; i < words.length; i++) {
                var word = words[i];
                words[i] = word.charAt(0).toLowerCase() + word.slice(1);
            }
            return words.join("-");
        },
        addToCart(product) {
            this.$store.dispatch('cart/addToCart', product)
        }
    },
    mounted() {
        this.loadProducts();
    }
}
</script>

<style>
button:focus{
    outline: none;
}
button:hover svg{
    color: white;
}
.btn{
    -webkit-box-align: center;
    align-items: center;
    border-radius: 4px;
    display: -webkit-inline-box;
    display: inline-flex;
    -webkit-box-flex: 0;
    flex: 0 0 auto;
    font-weight: 500;
    letter-spacing: .0892857143em;
    -webkit-box-pack: center;
    justify-content: center;
    max-width: 100%;
    outline: 0;
    position: relative;
    text-decoration: none;
    text-indent: .0892857143em;
    text-transform: uppercase;
    -webkit-transition-duration: .28s;
    transition-duration: .28s;
    -webkit-transition-property: box-shadow,opacity,-webkit-transform;
    transition-property: box-shadow,opacity,-webkit-transform;
    transition-property: box-shadow,transform,opacity;
    transition-property: box-shadow,transform,opacity,-webkit-transform;
    -webkit-transition-timing-function: cubic-bezier(.4,0,.2,1);
    transition-timing-function: cubic-bezier(.4,0,.2,1);
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    vertical-align: middle;
    white-space: nowrap;
    box-shadow: 0 3px 1px -2px rgba(0,0,0,.2), 0 2px 2px 0 rgba(0,0,0,.14), 0 1px 5px 0 rgba(0,0,0,.12);
}
.shadow-manual:hover{
   box-shadow: 0 1px 7px 2px rgb(0 0 0 / 10%);
}
.manual-img{
    padding: 25px 25px 0px 25px;
    transition: padding .5s;
}
/* #outer-div{
    outline:dashed;
    outline-offset: -25px;
} */
.shadow-manual:hover .manual-img{
    padding: 15px 15px 0px 15px;
}
</style>