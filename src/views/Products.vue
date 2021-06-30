<template>
<div class="container py-5">
  <div class="row mt-5">
    <template v-for="(item, key) in products" :key="item.id">
      <div class="col-12" v-if="key % 4 === 0">
        <a href="#" class="text-dark text-decoration-none" @click.prevent="goToProduct(item)">
          <div class="product mb-3">
            <div class="row align-items-center">
              <div class="col-md-6">
                <div class="bg-lg" :style="`background-image: url(${item.imageUrl})`">
                </div>
              </div>
              <div class="col-md-6 p-5">
                <p class="d-none d-md-block">{{ item.category }}</p>
                <h2>{{ item.title }}</h2>
                <p>$ {{ item.price }}</p>
                <p class="my-5 d-none d-md-block">Lorem ipsum dolor
                  sit amet consectetur adipisicing elit. Atque,
                  eligendi.Lorem ipsum dolor sit amet consectetur
                  adipisicing elit. Atque, eligendi.</p>
                <button type="button"
                class="btn button rounded-0 d-none d-md-block w-50">查看更多</button>
              </div>
            </div>
          </div>
        </a>
      </div>
      <div class="col-md-4" v-else>
        <a href="#" class="text-dark text-decoration-none" @click.prevent="goToProduct(item)">
          <div class="product mb-3">
            <div>
              <div class="bg" :style="`background-image: url(${item.imageUrl})`"></div>
              <div class="text p-5">
                <h2>{{ item.title }}</h2>
                <p>$ {{ item.price }}</p>
              </div>
            </div>
          </div>
        </a>
      </div>
    </template>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      loadingStatus: {
        isLoading: '',
      },
      products: [],
    };
  },
  methods: {
    getProducts() {
      this.$http
        .get(`${process.env.VUE_APP_URL}/api/${process.env.VUE_APP_PATH}/products`)
        .then((res) => {
          console.log(res.data);
          if (res.data.success) {
            this.products = res.data.products;
          } else {
            console.log(res.data.message);
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
    goToProduct(item) {
      this.$router.push(`/product/${item.id}`);
    },
  },
  mounted() {
    this.getProducts();
  },
};
</script>

<style lang="scss" scoped>
.product {
  transition: .3s;
}
a:hover .product{
  box-shadow: 1px 1px 10px 0px rgba(0, 0, 0, 0.2);
}

@mixin pad {
  @media (max-width: 768px) {
    @content;
  }
}
.product {
  background-color: #eee;
}
a:hover .child {
  transform: scale(1.2);
}
.bg-lg {
  height: 500px;
  background-size: cover;
  background-position: center;
  @include pad {
    height: 300px;
  }
}
.bg {
  height: 300px;
  background-size: cover;
  background-position: center;
}

.button {
  text-decoration: none;
  padding: 1rem;
  background-color: #fff;
  color: #000;
  position: relative;
  z-index: 10;
  &:hover {
    color: #fff;
  }
  &::before {
    transition: 0.3s ease-in-out;
    content: "";
    position: absolute;
    left: -1px;
    top: 0;
    width: 0px;
    height: 100%;
    background-color: #000;
    z-index: -1;
  }
  &:hover::before {
    content: "";
    left: 0;
    top: 0;
    width: 100%;
  }
}
.bg-cover {
  background-size: cover;
  background-position: center center;
}
</style>
