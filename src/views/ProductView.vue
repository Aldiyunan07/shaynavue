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
                                    <p>
                                        {{ productDetails.description }}
                                    </p>
                                    <h4>${{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/cart" class="primary-btn pd-cart">Add To Cart</router-link>
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
        gambar_default:"",
        thumbs : [
          "img/products/man-1.jpg",
          "img/products/man-2.jpg",
          "img/products/man-3.jpg",
          "img/products/man-4.jpg"
        ],
        productDetails : []
    }
    },
    methods: {
      changeImage(urlImage){
        this.gambar_default = urlImage;
      },
        setDataPicture(data){
            this.productDetails = data;
            this.gambar_default = data.gallery[0].photo;
        },
    },
    mounted(){
        axios
        .get("http://localhost:8000/api/products",{
            params:{
                id: this.$route.params.id
            }
        })
        .then(res => (this.setDataPicture(res.data.meta.data)))
        .catch(err => console.log(err));
    }
  }
</script>
<style scoped>
  .product-thumbs .pt{
    margin-right: 10px;
  }
</style>