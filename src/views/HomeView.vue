
<script>
import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';

export default {
name: "HomeView",
data() {
return {
books: []
}
},
components: {
Header,
Footer
},
mounted() {
fetch("http://127.0.0.1:8000/api/books?page=1")
.then(res => res.json())
.then(data => this.books = data, this.pages)
.catch(err => console.log(err.message));
},
methods: {
rep(str) {
str = str.replace(new RegExp("\\\\", "g"), "");
return str;
},
fetchData(page) {
if (page <= this.books.total_pages && page > 0) {
fetch("http://127.0.0.1:8000/api/books?page=" + page)
.then(res => res.json())
.then(data => this.books = data, this.pages)
.catch(err => console.log(err.message));
}
},
getActiveClass(page) {
return page == this.books.page ? "active" : ""
},
getDisableNextClass(page) {
return page == this.books.total_pages ? "disabled" : ""
},
getDisablePreviousClass(page) {
return page == 1 ? "disabled" : ""
},
}
}
</script>
<template>
  <Header />
  <!-- Products Start -->
  <div class="container-fluid pt-5 pb-3">
    <h2 class="section-title position-relative text-uppercase mx-xl-5 mb-4"><span class="bg-secondary pr-3">Books</span>
    </h2>
    <div class="row px-xl-5">

      <div v-for="book in books.data" :key="book.id" class="col-lg-3 col-md-4 col-sm-6 pb-1">
        <div class="product-item bg-light mb-4">
          <div class="product-img position-relative overflow-hidden">
            <img class="img-fluid w-100" :src="rep(book.image)" :alt="book.title">
            <!-- <div class="product-action">
              <a class="btn btn-outline-dark btn-square" href=""><i class="fa fa-shopping-cart"></i></a>
              <a class="btn btn-outline-dark btn-square" href=""><i class="far fa-heart"></i></a>
              <a class="btn btn-outline-dark btn-square" href=""><i class="fa fa-sync-alt"></i></a>
              <a class="btn btn-outline-dark btn-square" href=""><i class="fa fa-search"></i></a>
            </div> -->
          </div>
          <div class="text-center py-4">
            <a class="h5 text-decoration-none text-truncate" href="">{{book.title}}</a>
            <div class="d-flex align-items-center justify-content-center mt-2">
              <h6>{{book.author}}</h6>
              <h6 class="text-muted ml-2">{{book.genre}}</h6>
            </div>
            <div class="d-flex align-items-center justify-content-center mb-1">
              <h6>{{book.publisher}}</h6>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="clearfix">
      <div class="hint-text">Showing <b>{{books.max_size}}</b> out of <b>{{books.total}}</b> entries</div>
      <ul class="pagination">
        <li class="page-item" v-bind:class="getDisablePreviousClass(books.page)" v-on:click="fetchData(--books.page)"><a class="page-link">Previous</a></li>
        <li v-for="n in books.total_pages" class="page-item" v-bind:class="getActiveClass(n)" v-on:click="fetchData(n)">
          <a class="page-link">{{n}}</a></li>
        <li class="page-item" v-bind:class="getDisableNextClass(books.page)" v-on:click="fetchData(++books.page)"><a class="page-link">Next</a></li>
      </ul>
    </div>
  </div>
  <!-- Products End -->
  <Footer />
</template>

<style scoped>
      .pagination {
        float: right;
        margin: 0 0 5px;
    }
    .pagination li a {
        border: none;
        font-size: 13px;
        min-width: 30px;
        min-height: 30px;
        color: #999;
        margin: 0 2px;
        line-height: 30px;
        border-radius: 2px !important;
        text-align: center;
        padding: 0 6px;
    }
    .pagination li a:hover {
        color: #666;
    }	
    .pagination li.active a, .pagination li.active a.page-link {
        background: #03A9F4;
    }
    .pagination li.active a:hover {        
        background: #0397d6;
    }
	.pagination li.disabled i {
        color: #ccc;
    }
    .pagination li i {
        font-size: 16px;
        padding-top: 6px
    }
    .hint-text {
        float: left;
        margin-top: 10px;
        font-size: 13px;
    }    
    .pagination .disabled{
        display: none !important;
    }
</style>