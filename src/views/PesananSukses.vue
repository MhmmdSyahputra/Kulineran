<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row justify-content-center mt-4">
        <div class="col text-center">
          <h2><strong>Sukses!!</strong></h2>
          <img src="../assets/images/logo-wait.png" width="350px" alt="" />
          <h4>
            Pesanan Anda Segera Diproses <br />
            Selamat Menunggu
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "PesananSukses",
  components: {
    Navbar,
  },
};
</script>
