<template>
  <b-container class="text-center d-flex flex-column align-items-center justify-content-center" style="height: 100%">
    <div>
      <h1 class="title text-center">Welcome on μPKI administration!</h1>
      <div class="logo mt-5 mb-5"></div>
      <div>
        <b-button type="button" variant="success" size="lg" @click="downloadCA()">
          <i class="fa fa-download"></i>
          Download CA certificate (crt file)
        </b-button>

        <b-button type="button" variant="primary" size="lg" class="ml-3" @click="downloadCRL()">
          <i class="fa fa-download"></i>
          Download CRL (pem file)
        </b-button>
      </div>
    </div>
  </b-container>
</template>

<script>
import request from './../core/request';

export default {
  name: "Home",
  data() {
    return {
      api_host: this.$root.publicAPI
    };
  },
  methods: {
    downloadCA() {
      request
        .get(`${this.api_host}/certs/ca.crt`)
        .then(res => {
          const variant = res.data.status === "success" ? "success" : "danger";
          if (variant === "success") {
            this.$root.forceFileDownload({
              name: "ca.crt",
              data: res.data.certificate
            });
          } else {
            this.$root.showAlert(res.data.message, "danger");
          }
        })
        .catch(error => {
          // eslint-disable-next-line
          console.error(error);
          this.$root.showAlert(error, "danger");
        });
    },
    downloadCRL() {
      request
        .get(`${this.api_host}/certs/crl.pem`)
        .then(res => {
          const variant = res.data.status === "success" ? "success" : "danger";
          if (variant === "success") {
            this.$root.forceFileDownload({
              name: "crl.pem",
              data: res.data.certificate
            });
          } else {
            this.$root.showAlert(res.data.message, "danger");
          }
        })
        .catch(error => {
          // eslint-disable-next-line
          console.error(error);
          this.$root.showAlert(error, "danger");
        });
    }
  }
};
</script>

<style lang="sass">
h1.title
  color: #3d4c5a

.logo
  background: url('~@/assets/logo.png') no-repeat center center
  background-size: contain
  margin: 0 auto
  width: 200px
  height: 200px

</style>
