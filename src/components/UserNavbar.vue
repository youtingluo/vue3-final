<template>
  <nav class="navbar navbar-expand-lg sticky-top navbar-light bg-light py-3">
    <div class="container">
      <div>
        <router-link class="navbar-brand font" to="/">
          <img class="logo" src="../assets/logo.svg" alt="logo">
          <span class="ms-3 fw-bold">COFFEE BEAT</span></router-link>
      </div>
      <button class="navbar-toggler" type="button"
      data-bs-toggle="collapse" data-bs-target="#navbarNav"
      aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse justify-content-end" id="navbarNav">
        <ul class="list d-lg-flex align-items-center my-3 my-lg-0">
          <li class="mb-3 mb-lg-0"><router-link to="/products"
          class="btn btn-outline-primary text-decoration-none rounded-pill
          d-block d-lg-inline-block">
          <span class="material-icons align-middle me-2">
          list
          </span>產品列表</router-link></li>
          <li class="mb-3 mb-lg-0"><router-link to="/carts"
          class="btn btn-outline-primary text-decoration-none
          rounded-pill ms-lg-3 d-block d-lg-inline-block">
          <span class="material-icons align-middle me-2">
              shopping_cart
              </span>購物車</router-link></li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css2?family=Viaoda+Libre&display=swap');
.font {
  font-family: 'Viaoda Libre', cursive;
}
</style>
