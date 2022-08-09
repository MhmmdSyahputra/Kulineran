<template>
  <div>
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- breadcrumb -->
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>

          <div class="table-responsive m-3">
            <table class="table table-striped">
              <thead>
                <tr align="center" class="bg-dark text-light">
                  <th scope="col">No</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody v-if="keranjangs.length">
                <tr
                  align="center"
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'../assets/images/' + keranjang.products.gambar"
                      class="img-fluid"
                      width="250"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>
                    {{ keranjang.jumlah }}
                  </td>
                  <td>
                    Rp.
                    {{ keranjang.products.harga }}
                  </td>
                  <td>
                    <strong>
                      Rp.
                      {{ keranjang.products.harga * keranjang.jumlah }}
                    </strong>
                  </td>
                  <td>
                    <button
                      class="btn btn-danger"
                      @click="hapusKeranjang(keranjang.id)"
                    >
                      <b-icon-trash></b-icon-trash>
                    </button>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right"><strong>Total :</strong></td>
                  <td align="center">
                    <strong>Rp. {{ totalHarga }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
              <tbody v-else>
                <tr>
                  <td colspan="8" align="center">
                    <h3>Keranjang Anda masih Kosong</h3>
                    <h4>
                      Silahkan Pesan
                      <router-link to="/foods" class="btn btn-success"
                        >Disini</router-link
                      >
                    </h4>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="row justify-content-end" v-if="keranjangs.length">
        <div class="col-md-4">
          <form v-on:submit.prevent>
            <div class="form-group">
              <label>Nama</label>
              <input type="text" class="form-control" v-model="pesan.nama" />
            </div>
            <div class="form-group">
              <label>Nomor Meja</label>
              <input
                type="number"
                class="form-control"
                v-model="pesan.Nomeja"
              />
            </div>

            <button class="btn btn-success float-right" @click="checkout">
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
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },

    hapusKeranjang(id) {
      axios
        .delete("https://my-json-server.typicode.com/MhmmdSyahputra/api-kulineran/keranjangs/" + id)
        .then(() => {
          this.$toast.error("Sukses Hapus Keranjang.", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
          axios
            .get("https://my-json-server.typicode.com/MhmmdSyahputra/api-kulineran/keranjangs")
            .then((response) => this.setKeranjangs(response.data))
            .catch((error) => console.log(error));
        })
        .catch((err) => console.log(err));
    },
    checkout() {
      if (this.pesan.nama && this.pesan.Nomeja) {
        this.pesan.keranjangs = this.keranjangs;
        axios
          .post("https://my-json-server.typicode.com/MhmmdSyahputra/api-kulineran/pesanans/", this.pesan)
          .then(() => {
            // Hapus Semua keranjang
            this.keranjangs.map(function (item) {
              return axios
                .delete("https://my-json-server.typicode.com/MhmmdSyahputra/api-kulineran/keranjangs/" + item.id)
                .catch((err) => console.log(err));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Sukses Di Pesan.", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Nama dan Nomor Meja Harus Diisi", {
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
      .get("https://my-json-server.typicode.com/MhmmdSyahputra/api-kulineran/keranjangs")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah;
      }, 0);
    },
  },
};
</script>
