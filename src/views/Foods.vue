<template>
  <div>
    <Navbar />
    <div class="container mt-5">
      <h2>Daftar <strong>Makanan</strong></h2>

      <div class="row mt-4">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="search"
              type="text"
              class="form-control"
              placeholder="Cari Makanan yang anda suka.."
              aria-label="cari"
              aria-describedby="basic-addon1"
              @keyup="searchFood"
            />
            <span class="input-group-text" id="basic-addon1"
              ><b-icon-search></b-icon-search
            ></span>
          </div>
        </div>
      </div>

      <div v-if="products.length">
        <div class="row mb-3">
          <div
            class="col-md-4 mt-4"
            v-for="product in products"
            :key="product.id"
          >
            <CardProduct :product="product" />
          </div>
        </div>
      </div>
      <div v-else>
        <div class="row">
          <div class="col">
            <h3 align="center">Makanan Tidak Ditemukan</h3>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import CardProduct from "@/components/Product.vue";
import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    searchFood() {
      axios
        .get("https://my-json-server.typicode.com/MhmmdSyahputra/api-kulineran/products?q=" + this.search)
        .then((response) => this.setProducts(response.data))
        .catch((error) => console.log(error));
    },
  },

  mounted() {
    axios
      .get("https://my-json-server.typicode.com/MhmmdSyahputra/api-kulineran/products")
      .then((response) => this.setProducts(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
