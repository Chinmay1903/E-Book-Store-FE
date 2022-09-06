<template>
    <div class="container">
        <div class="table-wrapper">
            <div class="table-title">
                <div class="row">
                    <div class="col-sm-6">
                        <h2 class="text-light">Manage <b>Books</b></h2>
                    </div>
                    <div class="col-sm-6">
                        <a href="#addBookModal" data-toggle="modal" class="btn btn-success" @click="addBookModal()"><i
                                class="material-icons">&#xE147;</i> <span>Add New Book</span></a>
                        <!-- <a href="#deleteBookModal" class="btn btn-danger" ><i
                                class="material-icons">&#xE15C;</i> <span>Delete</span></a> -->
                    </div>
                </div>
            </div>
            <table class="table table-striped table-hover">
                <thead>
                    <tr>
                        <th></th>
                        <th>Title</th>
                        <th>Author</th>
                        <th>Genre</th>
                        <th>Description</th>
                        <th>Isbn</th>
                        <th>Published</th>
                        <th>Publisher</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="book in books.data">
                        <td><img class="img-fluid w-100" :src="rep(book.image)" :alt="book.title"></td>
                        <td>{{book.title}}</td>
                        <td>{{book.author}}</td>
                        <td>{{book.genre}}</td>
                        <th>{{book.description}}</th>
                        <th>{{book.isbn}}</th>
                        <th>{{book.published}}</th>
                        <th>{{book.publisher}}</th>
                        <td>
                            <a href="#editBookModal" data-toggle="modal" @click="editbookModal(book.id)" class="edit" ><i class="material-icons" title="Edit">&#xE254;</i></a>
                            <a href="#deleteBookModal" data-toggle="modal" @click="deletebookModal(book.id)" class="delete" ><i class="material-icons" title="Delete">&#xE872;</i></a>
                        </td>
                    </tr>
                </tbody>
            </table>
            <div class="clearfix">
                <div class="hint-text">Showing <b>{{books.max_size}}</b> out of <b>{{books.total}}</b> entries</div>
                <ul class="pagination">
                    <li class="page-item" v-bind:class="getDisablePreviousClass(books.page)" v-on:click="fetchData(--books.page)"><a class="page-link">Previous</a></li>
                    <li v-for="n in books.total_pages" class="page-item" v-bind:class="getActiveClass(n)" v-on:click="fetchData(n)"><a class="page-link">{{n}}</a></li>
                    <li class="page-item" v-bind:class="getDisableNextClass(books.page)" v-on:click="fetchData(++books.page)"><a class="page-link">Next</a></li>
                </ul>
            </div>
        </div>
    </div>
    <!-- Add Modal HTML -->
    <div id="addBookModal" ref="modal" class="modal">
        <div class="modal-dialog">
            <div class="modal-content">
                <form>
                    <div class="modal-header">
                        <h4 class="modal-title">Add Book</h4>
                        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                    </div>
                    <div class="modal-body">
                        <div class="form-group">
                            <label>Title</label>
                            <input type="text" v-model="title" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Author</label>
                            <input type="text" v-model="author" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Genre</label>
                            <input type="text" v-model="genre" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Isbn</label>
                            <input type="text" v-model="isbn" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Published</label>
                            <input type="text" v-model="published" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Publisher</label>
                            <input type="text" v-model="publisher" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Description</label>
                            <textarea class="form-control" v-model="description" required></textarea>
                        </div>
                        <div class="form-group">
                            <label>Image</label>
                            <input type="file" class="form-control" @change="onFileSelect()">
                        </div>
                    </div>
                    <div class="modal-footer">
                        <input type="button" class="btn btn-default" data-dismiss="modal" value="Cancel">
                        <input type="submit" class="btn btn-success" @click="addNewBook()" value="Add">
                    </div>
                </form>
            </div>
        </div>
    </div>
    <!-- Edit Modal HTML -->
    <div id="editBookModal" class="modal">
        <div class="modal-dialog">
            <div class="modal-content">
                <form>
                    <div class="modal-header">
                        <h4 class="modal-title">Edit Book</h4>
                        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                    </div>
                    <div class="modal-body">
                        <div class="form-group">
                            <label>Title</label>
                            <input type="text" v-model="title" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Author</label>
                            <input type="text" v-model="author" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Genre</label>
                            <input type="text" v-model="genre" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Isbn</label>
                            <input type="text" v-model="isbn" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Published</label>
                            <input type="text" v-model="published" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Publisher</label>
                            <input type="text" v-model="publisher" class="form-control" required>
                        </div>
                        <div class="form-group">
                            <label>Description</label>
                            <textarea class="form-control" v-model="description" required></textarea>
                        </div>
                        <div class="form-group">
                            <label>Image</label>
                            <input type="file" class="form-control" @change="onFileSelect()">
                        </div>
                    </div>
                    <div class="modal-footer">
                        <input type="button" class="btn btn-default" data-dismiss="modal" value="Cancel">
                        <input type="submit" class="btn btn-info" @click="editBook(id)" value="Save">
                    </div>
                </form>
            </div>
        </div>
    </div>
    <!-- Delete Modal HTML -->
    <div id="deleteBookModal" class="modal">
        <div class="modal-dialog">
            <div class="modal-content">
                <form>
                    <div class="modal-header">
                        <h4 class="modal-title">Delete Book</h4>
                        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                    </div>
                    <div class="modal-body">
                        <p>Are you sure you want to delete the Book?</p>
                        <p class="text-warning"><small>This action cannot be undone.</small></p>
                    </div>
                    <div class="modal-footer">
                        <input type="button" class="btn btn-default" data-dismiss="modal" value="Cancel">
                        <input type="submit" class="btn btn-danger" @click="deletebook(id)" value="Delete">
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>    
    import axios from "axios";
    export default {
    name: "CRUDTable",
    data() {
    return {
    books: [],
    id:null,
    title:"",
    author:"",
    genre:"",
    description:"",
    isbn:"",
    published:"",
    publisher:"",
    image:""
        }
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
        getActiveClass(page){
            return page == this.books.page?"active":""
        },
        getDisableNextClass(page){
            return page == this.books.total_pages?"disabled":""
        },
        getDisablePreviousClass(page){
            return page == 1?"disabled":""
        },
        onFileSelect(e){
            this.image = e.target.files[0];
            console.log(this.image);
        },
        addBookModal(){
            this.id = null
            this.title = ""
            this.author = ""
            this.genre = ""
            this.description = ""
            this.isbn = ""
            this.published = ""
            this.publisher = ""
               },
        editbookModal(id){
            this.books.data.forEach(book => {
                if (book.id === id) {
                    this.id = book.id;
                    this.title = book.title;
                    this.author = book.author;
                    this.genre = book.genre;
                    this.description = book.description;
                    this.isbn = book.isbn;
                    this.published = book.published;
                    this.publisher = book.publisher;
                }
            });
        },
        addNewBook(){

        },
        editBook(id){
            debugger;
            console.log(id);
            const formData = new FormData();
            formData.append("title", this.title);
            formData.append("author", this.author);
            formData.append("genre", this.genre);
            formData.append("description", this.description)
            formData.append("isbn", this.isbn);
            formData.append("published", this.published)
            formData.append("publisher", this.publisher);
            formData.append("image", this.image)
            axios.put('http://127.0.0.1:8000/api/books/'+this.id, formData)
            .then(res => {
                console.log(res);
            })
        },
        deletebook(id){
            debugger;
            console.log(id);
            axios.delete('http://127.0.0.1:8000/api/books/'+this.id)
            .then(res => {
                console.log(res);
            })
        }
    }
}
</script>

<style scoped>
    body {
        color: #566787;
		background: #f5f5f5;
		font-family: 'Varela Round', sans-serif;
		font-size: 13px;
	}
	.table-wrapper {
        background: #fff;
        padding: 20px 25px;
        margin: 30px 0;
		border-radius: 3px;
        box-shadow: 0 1px 1px rgba(0,0,0,.05);
    }
	.table-title {        
		padding-bottom: 15px;
		background: #435d7d;
		color: #fff;
		padding: 16px 30px;
		margin: -20px -25px 10px;
		border-radius: 3px 3px 0 0;
    }
    .table-title h2 {
		margin: 5px 0 0;
		font-size: 24px;
	}
	.table-title .btn-group {
		float: right;
	}
	.table-title .btn {
		color: #fff;
		float: right;
		font-size: 13px;
		border: none;
		min-width: 50px;
		border-radius: 2px;
		border: none;
		outline: none !important;
		margin-left: 10px;
	}
	.table-title .btn i {
		float: left;
		font-size: 21px;
		margin-right: 5px;
	}
	.table-title .btn span {
		float: left;
		margin-top: 2px;
	}
    table.table tr th, table.table tr td {
        border-color: #e9e9e9;
		padding: 12px 15px;
		vertical-align: middle;
    }
	table.table tr th:first-child {
		width: 60px;
	}
	table.table tr th:last-child {
		width: 100px;
	}
    table.table-striped tbody tr:nth-of-type(odd) {
    	background-color: #fcfcfc;
	}
	table.table-striped.table-hover tbody tr:hover {
		background: #f5f5f5;
	}
    table.table th i {
        font-size: 13px;
        margin: 0 5px;
        cursor: pointer;
    }	
    table.table td:last-child i {
		opacity: 0.9;
		font-size: 22px;
        margin: 0 5px;
    }
	table.table td a {
		font-weight: bold;
		color: #566787;
		display: inline-block;
		text-decoration: none;
		outline: none !important;
	}
	table.table td a:hover {
		color: #2196F3;
	}
	table.table td a.edit {
        color: #FFC107;
    }
    table.table td a.delete {
        color: #F44336;
    }
    table.table td i {
        font-size: 19px;
    }
	table.table .avatar {
		border-radius: 50%;
		vertical-align: middle;
		margin-right: 10px;
	}
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
	/* Custom checkbox */
	.custom-checkbox {
		position: relative;
	}
	.custom-checkbox input[type="checkbox"] {    
		opacity: 0;
		position: absolute;
		margin: 5px 0 0 3px;
		z-index: 9;
	}
	.custom-checkbox label:before{
		width: 18px;
		height: 18px;
	}
	.custom-checkbox label:before {
		content: '';
		margin-right: 10px;
		display: inline-block;
		vertical-align: text-top;
		background: white;
		border: 1px solid #bbb;
		border-radius: 2px;
		box-sizing: border-box;
		z-index: 2;
	}
	.custom-checkbox input[type="checkbox"]:checked + label:after {
		content: '';
		position: absolute;
		left: 6px;
		top: 3px;
		width: 6px;
		height: 11px;
		border: solid #000;
		border-width: 0 3px 3px 0;
		transform: inherit;
		z-index: 3;
		transform: rotateZ(45deg);
	}
	.custom-checkbox input[type="checkbox"]:checked + label:before {
		border-color: #03A9F4;
		background: #03A9F4;
	}
	.custom-checkbox input[type="checkbox"]:checked + label:after {
		border-color: #fff;
	}
	.custom-checkbox input[type="checkbox"]:disabled + label:before {
		color: #b8b8b8;
		cursor: auto;
		box-shadow: none;
		background: #ddd;
	}
	/* Modal styles */
	.modal .modal-dialog {
		max-width: 400px;
	}
	.modal .modal-header, .modal .modal-body, .modal .modal-footer {
		padding: 20px 30px;
	}
	.modal .modal-content {
		border-radius: 3px;
	}
	.modal .modal-footer {
		background: #ecf0f1;
		border-radius: 0 0 3px 3px;
	}
    .modal .modal-title {
        display: inline-block;
    }
	.modal .form-control {
		border-radius: 2px;
		box-shadow: none;
		border-color: #dddddd;
	}
	.modal textarea.form-control {
		resize: vertical;
	}
	.modal .btn {
		border-radius: 2px;
		min-width: 100px;
	}	
	.modal form label {
		font-weight: normal;
	}	

</style>