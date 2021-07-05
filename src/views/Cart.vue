<template>
  <section class="py-5">
    <div class="container">
      <h2 class="text-center">您的購物車</h2>
      <!-- 購物流程 -->
      <div class="d-flex justify-content-center my-3">
        <div class="progresses">
          <div class="steps bg-primary">
            <span class="font-weight-bold">1</span>
          </div>
          <span class="line bg-primary"></span>
          <div class="steps bg-primary">
            <span class="font-weight-bold">2</span>
          </div>
          <span class="line bg-primary"></span>
          <div class="steps bg-primary"><span class="font-weight-bold">3</span></div>
        </div>
      </div>
      <!-- 購物流程 END -->
      <div class="row">
        <div class="col-lg-8">
          <table class="table text-primary">
            <thead class="text-dark bg-secondary">
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
              <tr
                class="align-middle"
                v-for="item in carts.carts"
                :key="item.id"
              >
                <td>
                  <p class="fw-bold">{{ item.product.title }}</p>
                </td>
                <td>
                  <div class="wrap">
                    <img :src="item.product.imageUrl" alt="product" />
                  </div>
                </td>
                <td>NT$ {{ item.product.price }}</td>
                <td>
                  <div class="btn-group">
                    <button type="button" class="btn btn-outline-primary">
                      -
                    </button>
                    <input
                      type="number"
                      :value="item.qty"
                      class="text-center"
                    />
                    <button type="button" class="btn btn-outline-primary">
                      +
                    </button>
                  </div>
                </td>
                <td>NT$ {{ item.total }}</td>
                <td class="discardBtn">
                  <a
                    href="#"
                    class="text-danger"
                    :class="{ disabled: icon.isLoading === item.id }"
                    @click.prevent="removeCartItem(item.id)"
                  >
                    <div
                      class="spinner-border spinner-border-sm"
                      role="status"
                      v-if="icon.isLoading === item.id"
                    >
                      <span class="visually-hidden"></span>
                    </div>
                    <span class="material-icons-outlined">
                      delete_outline
                    </span>
                  </a>
                </td>
              </tr>
            </tbody>
            <!-- <tfoot>
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
      </tfoot> -->
          </table>
          <div class="d-flex justify-content-between">
            <div class="btn btn-outline-primary">繼續購物</div>
            <div class="btn btn-primary">下一步</div>
          </div>
        </div>
        <div class="col-lg-4">
          <div class="order p-3 border border-3 bg-secondary">
            <h4>訂單列表</h4>
          </div>
        </div>
      </div>
    </div>
  </section>
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
  width: 120px;
  img {
    object-fit: cover;
    width: 100%;
    height: 100px;
  }
}
// 進度條
.progresses {
    display: flex;
    align-items: center
}

.line {
    width: 120px;
    height: 6px;
}

.steps {
    display: flex;
    color: #fff;
    font-size: 14px;
    width: 40px;
    height: 40px;
    align-items: center;
    justify-content: center;
    border-radius: 50%
}
</style>
