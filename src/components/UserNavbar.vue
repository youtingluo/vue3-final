<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light fixed-top">
    <div class="container">
      <router-link class="navbar-brand font" to="/">
      <img src="../assets/logo.svg" alt="logo" width="30">
      <span class="ms-3">COFFEE</span></router-link>
      <button class="navbar-toggler" type="button"
      data-bs-toggle="collapse" data-bs-target="#navbarNav"
      aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse justify-content-end" id="navbarNav">
        <ul class="navbar-nav align-items-center">
          <li class="nav-item">
            <router-link class="nav-link" to="/products">產品列表</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link fs-6" to="/carts" tabindex="-1"
              aria-disabled="true"><span class="material-icons align-middle">
              shopping_cart
              </span></router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Viaoda+Libre&display=swap');
.font {
  font-family: 'Viaoda Libre', cursive;
}
</style>
