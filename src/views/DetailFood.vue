<template>
  <div class="DetailFood">
    <Navbar />
    <div class="container">

      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Detail Food
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-4">
        <div class="col-md-6">
          <img
            :src="'../assets/images/' + product.gambar"
            class="img-fluid"
            alt=""
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong> {{ product.nama }} </strong>
          </h2>
          <hr />
          <h4>
            Harga : <strong>Rp. {{ product.harga }} </strong>
          </h4>
          <form v-on:submit.prevent>
            <div class="form-group">
              <label>Jumlah Pesan</label>
              <input
                type="number"
                class="form-control"
                v-model="pesan.jumlah"
              />
            </div>
            <div class="form-group">
              <label>Keterangan</label>
              <textarea
                v-model="pesan.keterangan"
                class="form-control"
                placeholder="Keterangan seperti : Pedas, Nasik Setengah"
              ></textarea>
            </div>
            <button class="btn btn-success" @click="pemesanan">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "DetailFood",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesan: {},
    };
  },
  methods: {
    setProducts(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesan.jumlah) {
        this.pesan.products = this.product;
        axios
          .post("http://localhost:3000/keranjangs/", this.pesan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Sukses Masuk Keranjang.", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Jumlah Pesanan Masih Kosong !", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },

  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => this.setProducts(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
