<template>
    <!-- Topbar Start -->
    <div class="container-fluid">
        <div class="row align-items-center bg-light py-3 px-xl-5 d-none d-lg-flex">
            <div class="col-lg-4">
                <img src=".\src\assets\img\book.png" alt="" srcset="">
                <a class="text-decoration-none">
                    <span class="h1 text-uppercase text-primary bg-dark px-2">Book</span>
                    <span class="h1 text-uppercase text-dark bg-primary px-2 ml-n1">Store</span>
                </a>
            </div>
            <div class="col-lg-8 text-left">
                <form action="">
                    <div class="input-group">
                        <input type="text" class="form-control" v-model="search" @keyup:enter="trigger"
                            placeholder="Search by Title, Author, Genre, ISBN, Publish Date, Publisher">
                        <div class="input-group-append">
                            <button type="button" @click="getSearch(search)" ref="search" class="input-group-text bg-transparent text-primary">
                                <i class="fa fa-search"></i>
                            </button>
                        </div>
                    </div>
                </form>
            </div>
            <!-- <div class="col-lg-4 col-6 text-right">
                <p class="m-0">Customer Service</p>
                <h5 class="m-0">+012 345 6789</h5>
            </div> -->
        </div>
    </div>
    <!-- Topbar End -->
</template>

<script>
export default {
name: "HomeView",
    data() {
        return {
        search: this.search,
        }
    },
    props: {
        search : String,
        getSearch : Function
    },
    methods:{
        trigger(){
            this.$refs.search.click()
        }
    }
}
</script>