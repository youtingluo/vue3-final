<template>
  <div class="banner">
  </div>
  <div class="container py-5">
      <!-- 產品 -->
      <div class="container my-5">
        <div class="row">
          <div class="col-5">
            <div v-if="JSON.stringify(this.product) === '{}'"
            class="spinner-border text-primary" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
            <img :src="product.imageUrl" :title="product.title" class="img-fluid" />
          </div>
          <div class="col-7">
            <h1>{{ product.title }}</h1>
            <span class="badge bg-info">{{ product.category }}</span>
            <p
              class="text-decoration-line-through"
            >
              原價 NT$ {{product.origin_price}}
            </p>
            <p>
              特價只要 NT$ {{product.price}}
            </p>
            <p>{{ product.description }}</p>
            <div class="d-flex">
              <select class="form-select w-50 me-3" aria-label="select qty" v-model.number="qty">
                <option selected disabled>選擇數量</option>
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
              </select>
              <button type="button" class="btn btn-primary"
              @click="addToCart(product.id)">加入購物車</button>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {},
      qty: 1,
    };
  },
  methods: {
    getProduct(id) {
      this.$http
        .get(`${process.env.VUE_APP_URL}/api/${process.env.VUE_APP_PATH}/product/${id}`)
        .then((res) => {
          if (res.data.success) {
            this.product = res.data.product;
          } else {
            console.log(res.data.message);
          }
        }).catch((err) => {
          console.log(err);
        });
    },
    addToCart(id) {
      const cart = { product_id: id, qty: this.qty };
      this.$http
        .post(`${process.env.VUE_APP_URL}/api/${process.env.VUE_APP_PATH}/cart`, { data: cart })
        .then((res) => {
          console.log(res);
          if (res.data.success) {
            console.log(res.data);
          } else {
            console.log(res.data.message);
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  mounted() {
    const { id } = this.$route.params;
    this.getProduct(id);
  },
};
</script>

<style lang="scss">
  .banner {
    width: 100%;
    min-height: 530px;
    background-image: url("https://images.unsplash.com/photo-1501346220112-1dbcb625301c?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80");
    background-position-y: 20px;
    background-size: cover;
  }
</style>
