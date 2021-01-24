<template>
  <div class="order_container">
    <h2 class="order_title">
      Order Summary
    </h2>
    <div class="order_promo">
      <label for="promo" class="promo">
        Apply Promo Code
      </label>
      <input
          id="promo"
          type="text"
          placeholder="Enter Your promocode"
          v-model="promocode"
      >
      <button
          class="promo_btn"
          v-on:click="$emit('apply-promo', promocode)"
      >
        Apply
      </button>
      <p class="promo_info">Code with higher savins will apply.</p>
      <hr>
    </div>
    <div class="order_total">
      <div class="order_reward">
        <span class="reward_title"><strong>Rewards:</strong></span>
        <span class="reward_count">-${{ calcTotalRewards().toFixed(2) }}</span>
      </div>
      <div>
        <span class="reward_info">
          Applied to 2 items
        </span>
      </div>
      <hr>
    </div>
    <div class="order_total">
      <div class="total">
        <span class="total_title">Items Total:</span>
        <span v-if="order.length === 0" class="total_value">$0.00</span>
        <span v-else class="total_value">${{(calculateCommonPrice() - calcTotalRewards()).toFixed(2)}}</span>
      </div>
      <div class="discount">
        <span class="discount_title"><strong>Discount:</strong></span>
        <span class="discount_value">-${{calculateDiscountPrice().toFixed(2)}}</span>
      </div>
      <hr>
    </div>
    <div class="order_subtotal">
      <div class="subtotal">
        <span  class="subtotal_title">Subtotal:</span>
        <span  class="subtotal_value">${{ calculateResultPrice().toFixed(2) }}</span>
      </div>
      <hr>
    </div>
    <div>
      <div class="order_result">
        <span class="result_title">Order Total:</span>
        <span class="order_price">${{ calculateResultPrice().toFixed(2)}}</span>
      </div>
    </div>
    <button class="checkout_btn">
      Checkout
    </button>
  </div>
</template>

<script>
  export default {
    props: ['order','trialProductId','reward'],
    data() {
      return {
        promocode : ''
      }
    },
    methods: {
      calculateCommonPrice() {
        return this.order.reduce((accum , item) => {
          return accum += item.price * item.count
        }, 0)
      },
      calculateDiscountPrice() {
        return this.order.reduce((accum , item) => {
          if(item.trialDiscount > 0 && item.productId === this.trialProductId) {
           return accum += item.trialDiscount * item.trialLimit
          } else if (item.discount > 0) {
            return accum +=  (item.price * item.discount / 100) * item.count
          }
          return accum
        },0)
      },
      calculateResultPrice() {
        return this.calculateCommonPrice() - this.calculateDiscountPrice()
      },
      calcTotalRewards() {
        let result = this.order.reduce((accum, i) => {
            return accum += this.reward * i.count
        }, 0)

        return result
      }
    }
  }
</script>

<style>
.order_container {
  padding: 32px;
  margin-left: 20px;
  margin-top: 48px;
  border-radius: 5px;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.15);
  width: 30%;
}

.order_title {
  margin: 0;
  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 24px;
  line-height: 32px;
  color: #3f3f3f;
}

.order_promo {
  margin-top: 16px;
}

.order_promo label {
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 20px;
  color: #3f3f3f;
}

.order_promo input {
  width: 56%;
  border-radius: 6px;
  border: 1px solid #cccccc;
  height: 44px;
  padding-left: 12px;
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 20px;
  color: #cccccc;
  opacity: 0.8;
}

.promo_btn {
  border: 1px solid #666666;
  border-radius: 6px;
  height: 44px;
  width: 37%;
  margin-left: 8px;
  background-color: transparent;
  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: #666666;
}

.promo_info {
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  line-height: 16px;
  color: #666666;
  margin-top: 8px;
}

.order_promo hr,
.order_total hr,
.order_subtotal hr {
  width: 100%;
  margin-top: 22px;
  background-color: #cccccc;
  height: 1px;
  border: none;
}

.order_total {
  display: flex;
  flex-direction: column;
  margin-top: 22px;
}

.total_title,
.total_value {
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 20px;
  color: #3f3f3f;
}

.discount {
  margin-top: 16px;
}

.discount_title,
.discount_value,
.reward_count{
  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
}

.discount_value,
.reward_count {
  color: #BD3C37;
}

.discount_title {
  color: #458500;
}

.order_subtotal {
  margin-top: 22px;
}

.subtotal_title,
.subtotal_value  {
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 20px;
  color: #3f3f3f;
}

.order_result {
  margin-top: 22px;
}

.result_title,
.order_price {
  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: #3f3f3f;
}

.checkout_btn {
  width: 100%;
  height: 44px;
  border-radius: 6px;
  background-color: #F38A00;
  border: transparent;
  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: #ffffff;
  margin-top: 22px;
}

.discount,
.total,
.subtotal,
.order_result,
.order_reward{
  display: flex;
  justify-content: space-between;
}

.promo {
  display: flex;
  flex-direction: column;
}

.reward_title {
  line-height: 24px;
  color: #458500;
}

.reward_info {
  font-size: 12px;
  line-height: 16px;
  color: #666666;
}

.reward {
  display: flex;
  justify-content: flex-end;
}

@media screen and (max-width: 940px){
  .order_promo input {
    width: 100%;
  }

  .promo_btn {
    width: 100%;
    margin: 20px 0 0;
  }

  .order_container {
    margin: auto;
    padding: 20px;
    width: 80%;
    margin-top: 20px;
  }
}


</style>