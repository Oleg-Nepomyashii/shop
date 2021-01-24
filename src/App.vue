<template>
  <div>
  <alertMessage
    v-on:toggle-alert="toggleAlertMessage"
    v-bind:isShowAlert="isShowAlert"
  />
  <section class="products_container">
    <table class="products_table">
      <thead class="products_head">
        <tr>
          <td><strong>Product(s)</strong></td>
          <td class="product_image"></td>
          <td></td>
          <td><strong>Price</strong></td>
          <td class="quantity_title"><strong>Quantity</strong></td>
          <td><strong>Total</strong></td>
        </tr>
      </thead>
        <productsTableList
            v-bind:products="products"
            v-bind:trialProductId="trialProductId"
            v-on:choose-trial="chooseTrial"
            v-on:order-choose="orderProduct"
            v-on:change-count="changeProductCount"
            v-bind:reward="reward"
            v-bind:order="order"
            v-on:toggle-alert="toggleAlertMessage"
        />
    </table>
      <order
          v-bind:order="order"
          v-bind:trialProductId="trialProductId"
          v-on:apply-promo="applyPromoCode"
          v-bind:reward="reward"
      />
  </section>
  </div>
</template>

<script>
import productsTableList from "@/components/productsTableList";
import order from "@/components/order";
import alertMessage from "@/components/alertTrialMessage"

export default {
  name: 'App',
  data() {
    return {
      products: [
        {
          "productId": 648739,
          "price": 27,
          "image" : "first.png",
          "discount": 0,
          "inventory": 9,
          "description": "product description",
          "trialLimit": 1,
          "trialDiscount": 10
        },
        {
          "productId": 648740,
          "price": 20,
          "image" : "",
          "discount": 0,
          "inventory": 5,
          "description": "product description 2",
          "trialLimit": 1,
          "trialDiscount": 8
        },
        {
          "productId": 648741,
          "price": 70,
          "image" : "",
          "discount": 10,
          "inventory": 3,
          "description": "product description 3",
        }
      ],
      trialProductId: null,
      order: [],
      reward: null,
      isShowAlert: false
    }
  },
  components: {
    productsTableList,
    order,
    alertMessage
  },
  methods: {
    chooseTrial(id) {
      this.trialProductId = id
    },
    orderProduct(product , count) {
      let a = this.order.findIndex( p => p.productId === product.productId)
      if(a === -1) {
        product.count = count
        this.order.push(product)
      } else {
        // let ind = this.order.findIndex(i => i.productId === product.productId)
        this.order = this.order.filter( i => i.productId !== product.productId)
      }
    },
    changeProductCount(id , count) {
      console.log(id)
      console.log(count)
      this.order = this.order.map(i => {
        if(i.productId === id) {
          i.count = Number(count)
          return i
        }
        return i
      })
    },
    applyPromoCode(promo) {
      // Тут я так понимаю должен пойти запрос на сервер
      // и проверить рабочий ли промокод и какую скидку он дает
      // Предположу что вводят правильный промокод 'AN39S1234'
      // который дает скидку 1 доллар на 1ед. товара
      if (promo === 'AN39S1234') {
        this.reward = 1
      } else {
        this.reward = null
      }
    },
    toggleAlertMessage() {
      this.isShowAlert = !this.isShowAlert
    }
  },
  created() {
    this.products.forEach(i => {
          if(this.trialProductId === null && i.trialDiscount) {
            return this.trialProductId = i.productId
          }
      })
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');


*,::before,::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-size: 16px;
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 500;
}

.products_container {
  max-width: 1280px;
  margin: auto;
  display: flex;
  padding: 80px 10px 0;
}

.products_head tr td{
  text-align: center;
  line-height: 24px;
  color: #000000;
}

.quantity_title {
  text-align: start!important;
}

.products_table {
  border-collapse: collapse;
}

.products_table tr td{
  padding-bottom: 9px;
}

.products_table td {
  padding: 15px 0;
  border-bottom: 1px solid #cccccc;
}

.try_active::after,
.try_not-active:after {
  content: '';
  display: inline-block;
  position: absolute;
  top: 100%;
  width: 0;
  height: 0;
  top: -2px;
  right: -3px;
  border-top: 12px solid transparent;
  border-bottom: 12px solid transparent;
  border-right: 14px solid #fff;
}

@media screen and (max-width: 940px) {
  .products_container {
    flex-direction: column;
  }
}

@media screen and (max-width: 640px){
  .product_image {
    display: none;
  }

  .trial_info {
    padding: 0;
  }
}

</style>
