<template>
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <Carousel class="product-slider" :items="3" :dots="false" :nav="false" :autoplay="true" >
                        <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
                            <div class="pi-pic">
                                <img v-bind:src="itemProduct.gallery[0].photo" alt="" />
                                <ul>
                                    <li class="w-icon active" @click="saveKeranjang(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.gallery[0].photo)">
                                        <a href="#"> <i class="icon_bag_alt"></i> </a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link v-bind:to="'/product/'+itemProduct.id">
                                            + Quick View
                                        </router-link>    
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name"> {{ itemProduct.type }} </div>
                                <route-link to="/product">
                                        <h5>{{ itemProduct.name }}</h5>
                                </route-link>
                                <div class="product-price">
                                    ${{ itemProduct.price }}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>
                    </Carousel>
                </div>
                <div class="col-lg-12" v-else>
                    <p> Produk baru tidak tersedia untuk saat ini </p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import axios from "axios";
import Carousel from 'vue-owl-carousel';
export default {
    name : "WomanShayna",
    components: {
        Carousel
    },
    data() {
        return {
            products : [],
            keranjangUser : [],
        };
    },    
    
    mounted(){
        axios
        .get("http://localhost:8000/api/products")
        .then(res => (this.products = res.data.meta.data.data))
        .catch(err => console.log(err));
        
        if (localStorage.getItem('keranjangUser')){ // Jika di get item dari local storage ada
            try{
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser')); // maka simpan data dari local storage ke variable keranjangUser
            }catch(e){
                localStorage.removeItem('keranjangUser'); // Ku tak tahu
            }
        }
    },

    methods:{
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct){ // function saveKeranjang
            var productStorage = { // Membuat variabel producStorage
                "id" : idProduct,
                "name" : nameProduct,
                "price" : priceProduct,
                "photo" : photoProduct
            }
            this.keranjangUser.push(productStorage); // Menambahkan data variabel productstorage ke var keranjanguser
            const parsed = JSON.stringify(this.keranjangUser); // me refresh data yang sudah di masukkan tadi
            localStorage.setItem('keranjangUser', parsed);
            window.location.reload();
        }

    }
};
</script>
<style scoped>
.product-item{
    margin-right:25px;
}
</style>