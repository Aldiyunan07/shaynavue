<template>
        <!-- Header Section Begin -->
        <header class="header-section">
            <div class="header-top">
                <div class="container">
                    <div class="ht-left">
                        <div class="mail-service">
                            <i class=" fa fa-envelope"></i> hello.shayna@gmail.com
                        </div>
                        <div class="phone-service">
                            <i class=" fa fa-phone"></i> +628 22081996
                        </div>
                    </div>
                </div>
            </div>
            <div class="container">
                <div class="inner-header">
                    <div class="row">
                        <div class="col-lg-2 col-md-2">
                            <div class="logo">
                                <a href="/">
                                    <img src="img/logo_website_shayna.png" alt="" />
                                </a>
                            </div>
                        </div>
                        <div class="col-lg-7 col-md-7"></div>
                        <div class="col-lg-3 text-right col-md-3">
                            <ul class="nav-right">
                                <li class="cart-icon">
                                    Keranjang Belanja &nbsp;
                                    <a href="#">
                                        <i class="icon_bag_alt"></i>
                                        <span>{{ keranjangUser.length }}</span>
                                    </a>
                                    <div class="cart-hover">
                                        <div class="select-items">
                                            <table>
                                                <tbody v-if="keranjangUser.length > 0">
                                                    <tr v-for="keranjang in keranjangUser" v-bind:key="keranjang.id">
                                                        <td class="si-pic">
                                                            <img class="photo-item" :src="keranjang.photo" alt="" />
                                                        </td>
                                                        <td class="si-text">
                                                            <div class="product-selected">
                                                                <p>${{ keranjang.price }} x 1 </p>
                                                                <h6>{{ keranjang.name }}</h6>
                                                            </div>
                                                        </td>
                                                        <td class="si-close" @click="removeItem(keranjang.id)">
                                                            <i class="ti-close"></i>
                                                        </td>
                                                    </tr>  
                                                </tbody>
                                                <tbody v-else>
                                                    <tr>
                                                        <td align="center"> Keranjang Kosong </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <div class="select-total">
                                            <span>total:</span>
                                            <h5>${{ totalHarga }}.00</h5>
                                        </div>
                                        <div class="select-button">
                                            <a href="#" class="primary-btn view-card"> <router-link to="/cart" style="color:white;"> VIEW CARD </router-link></a>                                            
                                            <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                                        </div>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </header>
</template>

<script>
    
    export default {
        name : "HeaderShayna",
        data() {
            return {
                keranjangUser :[] // Menyimpan data ke dalam variabel keranjangUser dari localstorage
                };
        },
        methods: {
            removeItem(idx){ // Function menghapus item dengan parameter index 
                // this.keranjangUser.splice(index, 1); // Memotong item dari local storage
                // const parsed = JSON.stringify(this.keranjangUser); // 2 line me refresh kembali perubahan yang sudah terjadi
                // localStorage.setItem('keranjangUser', parsed);

                // Cari Tahu id dari si item yang akan di hapus
                let keranjangUserStorage = JSON.parse(localStorage.getItem("keranjangUser")); // Get data keranjang 
                let itemKeranjangUserStorage = keranjangUserStorage.map(itemKeranjangUserStorage => itemKeranjangUserStorage.id); // Me listing kembali data array dan mendapatkan data yang di pilih
                
                // cocokan idx item dengan id yang di peroleh 
                let index = itemKeranjangUserStorage.findIndex(id => id == idx); // mencari data yang di maksud dengan menggunakan id yang di dapat
                this.keranjangUser.splice(index, 1); // Hapus data yang ada di dalam local storage
                // .splice(index, 1)
                // index = id dari si item yang akan di hapus 
                // 1 = jumlah data yang akan di hapus 

                // Update terkini dari local storage 
                const parsed = JSON.stringify(this.keranjangUser); // 2 line me refresh kembali perubahan yang sudah terjadi
                localStorage.setItem('keranjangUser', parsed);
                window.location.reload();

            }
        },
        mounted(){
            if (localStorage.getItem('keranjangUser')){ // Jika di get item dari local storage ada 
                try{
                    this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser')); // maka simpan data dari local storage ke variable keranjangUser
                }catch(e){
                    localStorage.removeItem('keranjangUser'); // Ku tak Tahu
                }
            }
        },
        computed:{ // Menampilkan data yang akan di kalkulasi kan nanti nya 
            totalHarga(){
                return this.keranjangUser.reduce(function(items, data){ // reduce Menghitung looping
                    return items + data.price;
                }, 0);   
            }
        }
    }
</script>
<style scoped>
.photo-item{
    width:80px;
    height:80px;
}
</style>