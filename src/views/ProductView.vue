<template>
  <div class="product">
    <HeaderShayna />
        <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link to="/"><i class="fa fa-home"></i> Home </router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="gambar_default" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.gallery.length > 0">
                                <Carousel :nav="false" :dots="false" class="product-thumbs-track ps-slider" >
                                    <div
                                    v-for="ss in productDetails.gallery" :key="ss.id"
                                    class="pt" @click="changeImage(ss.photo)" :class="ss.photo == gambar_default ? 'active' : ''" >
                                        <img :src="ss.photo" alt="" />
                                    </div>
                                </Carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description"></p>
                                    <h4>${{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/cart" class="primary-btn pd-cart">
                                        <a href="#" @click="saveKeranjang(productDetails.id, productDetails.name, productDetails.price, productDetails.gallery[0].photo)" class="primary-btn pd-cart"> Add To Cart </a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->
    <RelatedShayna/>
    <FooterShayna/>
  </div>
</template>

<script>
  // @ is an alias to /src
  // import HelloWorld from '@/components/HelloWorld.vue'
  import axios from "axios"
  import Carousel from 'vue-owl-carousel';
  import HeaderShayna from '@/components/HeaderShayna.vue'
  import RelatedShayna from '@/components/RelatedShayna.vue'
  import FooterShayna from '@/components/FooterShayna.vue'
  export default {
    name: 'ProductView',
    components: {
    HeaderShayna,
    FooterShayna,
    RelatedShayna,
    Carousel
    },
    data() {
      return {
        gambar_default:"",  // membuat variabel kosong 
        productDetails : [], // sama saja tapi bertipe array
        keranjangUser :[]
        };
    },
    methods: {
        changeImage(urlImage){ // Membuat function dengan get url image
            this.gambar_default = urlImage; // mendefinisikan variabel dengan url image
        },
        setDataPicture(data){ // Membuat function dengan set data picture
            this.productDetails = data; // mendefinisikan variabel productdetail dengan data hasil get
            this.gambar_default = data.gallery[0].photo; // mendefinisikan variabel dengan isi array dari get data
        },
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
        }
    },
    mounted(){
        if (localStorage.getItem('keranjangUser')){ // Jika di get item dari local storage ada
            try{
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser')); // maka simpan data dari local storage ke variable keranjangUser
            }catch(e){
                localStorage.removeItem('keranjangUser'); // Ku tak tahu
            }
        }
        axios
        .get("http://localhost:8000/api/products",{ // get data dari url
            params:{ // Mendefinisikan parameter
                id: this.$route.params.id  // mengisi parameter id dengan hasil get dari index
            }
        })
        .then(res => (this.setDataPicture(res.data.meta.data))) // get data dari API
        .catch(err => console.log(err)); // Jika terjadi error maka akan di beritahukan di console.log
    }
  }
</script>
<style scoped>
  .product-thumbs .pt{
    margin-right: 10px;
  }
</style>