<template>
    <section>
            <div class="container">
                <div class="row">
                    <h1 class="text-center mb-3">Toko ABCD</h1>
                    <div class="col-6">
                        <div class="mb-3">
                            <label for="name" class="form-label">Nama Product</label>
                            <input type="name" class="form-control" id="name" placeholder="Nama Produk" v-model="nama">
                        </div>
                    </div>
                    <div class="col-6">
                        <div class="mb-3">
                            <label for="price" class="form-label">Harga Produk</label>
                            <input type="price" class="form-control" id="price" placeholder="Harga Produk" v-model="harga">
                        </div>
                    </div>
                    <div class="col-6">
                        <button type="button" v-on:click="addProduct" class="btn btn-primary">Tambah Produk</button>
                        <button type="button" v-on:click="updateProduct" class="btn btn-success">Edit Produk</button>
                    </div>
                </div>
            </div>
            <hr>
            <div class="container">
                <div class="row">
                    <h1 class="text-center mb-3">Ready Stock</h1>
                    <div class="col-12">
                        <table class="table table-dark table-hover">
                            <thead>
                                <tr>
                                  <th scope="col">No</th>
                                  <th scope="col">Nama Produk</th>
                                  <th scope="col">Harga Produk</th>
                                  <th scope="col">Aksi</th>
                                </tr>
                              </thead>
                              <tbody>
                                <tr v-for="(product, ids) in products">
                                  <th scope="row">{{ ids+1 }}</th>
                                  <td>{{ product.nama }}</td>
                                  <td>{{ product.harga }}</td>
                                  <td>
                                      <button type="button" :data-id="product.id" v-on:click="editProduct" :data-nama="product.nama" :data-harga="product.harga" class="btn btn-primary">Edit</button>
                                      <button type="button" :data-id="product.id" v-on:click="deleteProduct" class="btn btn-danger">Hapus</button>
                                  </td>
                                </tr>
                              </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>
</template>
<script>
    const baseUrl = window.location.origin;

    export default {
        data() {
            return {
                products: '',
                harga: '',
                nama: '',
                id: ''
            }
        },

        mounted() {
            this.getProduct()
        },

        methods: {
            getProduct: function () {
                fetch(baseUrl+"/api/product/get")
                    .then(response => response.json())
                    .then(data => (
                        this.products = data
                    ));
            },

            addProduct: function () {
                if (this.nama !== '' && this.harga !== '') {
                    const requestOptions = {
                        method: 'POST',
                        headers: {
                        'Content-Type': 'application/json',
                        },
                        body: JSON.stringify(
                            { nama: this.nama, harga: this.harga },
                        )
                    };
                     fetch(baseUrl+"/api/product/add", requestOptions)
                        .then(response => response.json())
                        .then(data => this.products.push(data));
                }
            },

            deleteProduct: function (event) {
                var id = event.target.getAttribute('data-id');
                fetch(baseUrl+"/api/product/delete/"+id)
                    .then(response => response.json())
                    this.getProduct();
            },

            editProduct: function (event) {
                this.id = event.target.getAttribute('data-id');
                this.nama = event.target.getAttribute('data-nama');
                this.harga = event.target.getAttribute('data-harga');
            },

            updateProduct: function() {
                if (this.nama && this.harga  && this.id) {
                    const requestOptions = {
                        method: 'POST',
                        headers: {
                        'Content-Type': 'application/json',
                        },
                        body: JSON.stringify(
                            { nama: this.nama, harga: this.harga },
                        )
                    };
                     fetch(baseUrl+"/api/product/edit/"+this.id, requestOptions)
                        .then(response => response.json())
                    this.getProduct();
                    alert('update successfully')
                }
            }
        },

    }
</script>
