<template>
    <!-- 商品 -->
    <div class="wrap py-2 p-md-3" v-for="item in carts.carts" :key="item.id">
      <div class="d-flex">
        <div>
          <a href="#">
            <img :src="item.product.imageUrl" title="item.product.title" alt="product Image">
          </a>
        </div>
        <div class="d-flex flex-column justify-content-between ms-auto">
          <div>
            <h5>{{ item.product.title }}</h5>
          </div>
          <div class="d-flex justify-content-end align-items-center">
            <p class="mb-0">{{ item.qty }} x NT$ {{ item.product.price }}</p>
              <button type="button"
              class="btn btn-sm btn-outline-danger ms-3"
              @click.prevent="removeCarts(item.id)"
              >
                <span class="material-icons-outlined">
                  remove
                </span></button>
          </div>
        </div>
      </div>
    </div>
    <!-- <table class="table">
      <thead>
        <tr>
          <th>品項</th>
          <th></th>
          <th>單價</th>
          <th>數量</th>
          <th>金額</th>
          <th></th>
        </tr>
      </thead>
      <tbody class="js-cartItem">
        <tr v-for="item in carts.carts" :key="item.id">
        <td>
            <p>{{ item.product.title }}</p>
        </td>
        <td><img :src="item.product.imageUrl" alt="product" width="120" height="100"></td>
        <td>NT$ {{ item.product.price }}</td>
        <td>
          <a href="#" class="btn btn-danger btn-sm"
          :class="{'disabled':item.qty===1 ||icon.isLoading===item.id}"
          @click.prevent="updateCart(item,item.qty-1)">-</a>
          {{item.qty}}
          <a href="#" class="btn btn-danger btn-sm"
          :class="{'disabled' : icon.isLoading === item.id}"
          @click.prevent="updateCart(item,item.qty+1)">+</a>
        </td>
        <td>NT$ {{ item.total }}</td>
        <td class="discardBtn">
          <a href="#" class="btn btn-danger"
          :class="{'disabled' : icon.isLoading === item.id}"
          @click.prevent="removeCartItem(item.id)">
          <div
            class="spinner-border spinner-border-sm"
            role="status"
            v-if="icon.isLoading === item.id"
          >
            <span class="visually-hidden"></span>
          </div>刪除
          </a>
        </td>
      </tr>
      </tbody>
      <tfoot>
        <tr>
          <td>
            <a href="#" class="btn btn-outline-danger"
            @click.prevent="removeAllCarts()">刪除所有品項</a>
          </td>
          <td></td>
          <td></td>
          <td>
            <p>總金額</p>
          </td>
          <td class="text-info fw-bold">NT$ {{ carts.total }}</td>
        </tr>
      </tfoot>
    </table> -->
</template>

<script>
export default {
  data() {
    return {
      carts: {
        carts: [],
      },
      icon: {
        isLoading: '',
      },
    };
  },
  methods: {
    getCarts() {
      this.$http
        .get(`${process.env.VUE_APP_URL}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          if (res.data.success) {
            console.log(res);
            this.carts = res.data.data;
          } else {
            console.log(res);
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
    updateCart(item, qty) {
      this.icon.isLoading = item.id;
      const cart = { product_id: item.product_id, qty };
      this.$http
        .put(`${process.env.VUE_APP_URL}/api/${process.env.VUE_APP_PATH}/cart/${item.id}`, { data: cart })
        .then((res) => {
          if (res.data.success) {
            console.log(res.data.message);
            this.getCarts();
            this.icon.isLoading = '';
          } else {
            console.log(res.data.message);
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
    removeCarts(id) {
      this.icon.isLoading = id;
      this.$http
        .delete(`${process.env.VUE_APP_URL}/api/${process.env.VUE_APP_PATH}/cart/${id}`)
        .then((res) => {
          if (res.data.success) {
            this.icon.isLoading = '';
            this.getCarts();
          } else {
            console.log(res.data.message);
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
    removeAllCarts() {
      this.$http
        .delete(`${process.env.VUE_APP_URL}/api/${process.env.VUE_APP_PATH}/carts`)
        .then((res) => {
          if (res.data.success) {
            console.log(res.data.message);
            this.$router.push('/products');
          } else {
            console.log(res.data.message);
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  created() {
    this.getCarts();
  },
};
</script>

<style lang="scss">
  .wrap {
    width: 100%;
    img {
      object-fit: cover;
      width: 100px;
      height: 100px;
    }
  }
</style>
