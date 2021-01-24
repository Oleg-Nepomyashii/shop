<template>
  <tr>
    <td class="product_checkbox">
      <div
          class="product_box"
          v-bind:class="{
            ifChecked: checkIsChoose()
          }"
      >
        <input
            type="checkbox"
            value="None"
            :id="product.productId"
            name="check"
            v-on:change="$emit('order-choose', product , selectedCount)"
        />
        <label :for="product.productId"></label>
      </div>
    </td>
    <td class="product_image">
      <img src="@/assets/first.png" alt="image11">
    </td>

    <td class="product_top_align">
      <p v-if="product.trialDiscount" class="info">
        <button
            v-bind:class="isChooseTry(product.productId)"
            v-on:click="$emit('choose-trial' , product.productId)"
        >
          Try it
        </button>
        <svg
            v-on:click="$emit('toggle-alert')"
            v-if="isChooseTry(product.productId) === 'try_active'"
            class="quest_svg"
            width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
          <circle cx="8" cy="8" r="7.5" stroke="#666666"/>
          <path d="M7.2102 9.53226C7.2102 9.03047 7.2705 8.63082 7.39109 8.33333C7.51168 8.03584 7.73158 7.74373 8.05079 7.45699C8.37354 7.16667 8.58812 6.9319 8.69452 6.75269C8.80093 6.56989 8.85413 6.37814 8.85413 6.17742C8.85413 5.57168 8.57748 5.26882 8.02419 5.26882C7.76172 5.26882 7.55069 5.35125 7.39109 5.51613C7.23503 5.67742 7.15345 5.90143 7.14636 6.18817H5.60352C5.61061 5.50358 5.82874 4.96774 6.25789 4.58065C6.6906 4.19355 7.27936 4 8.02419 4C8.7761 4 9.35954 4.18459 9.77452 4.55376C10.1895 4.91935 10.397 5.43728 10.397 6.10753C10.397 6.41219 10.3296 6.70072 10.1948 6.97312C10.06 7.24194 9.82417 7.54122 9.48723 7.87097L9.0563 8.28495C8.78674 8.5466 8.63246 8.85305 8.59344 9.2043L8.57216 9.53226H7.2102ZM7.05592 11.1828C7.05592 10.9427 7.13572 10.7455 7.29532 10.5914C7.45848 10.4337 7.66596 10.3548 7.91778 10.3548C8.1696 10.3548 8.37532 10.4337 8.53492 10.5914C8.69807 10.7455 8.77965 10.9427 8.77965 11.1828C8.77965 11.4194 8.69984 11.6147 8.54024 11.7688C8.38418 11.9229 8.1767 12 7.91778 12C7.65887 12 7.44961 11.9229 7.29 11.7688C7.13395 11.6147 7.05592 11.4194 7.05592 11.1828Z" fill="#666666"/>
        </svg>
        <span v-if="isChooseTry(product.productId) === 'try_active'" class="active_applied">${{ product.trialDiscount.toFixed(2) }} off applied</span>
        <span v-else class="not_active_applied">Apply: ${{ product.trialDiscount.toFixed(2) }} Off</span>
      </p>
      <p class="product_description">
        Sierra Fit, Keto Shake, Chocolate, 1.49 lbs (675 g)
      </p>
      <p class="product_id">Product ID: {{ product.productId }}</p>
    </td>
    <td class="product_top_align">
      <span class="product_price">
        ${{ product.price.toFixed(2) }}
      </span>
    </td>
    <td class="product_top_align">
      <select
          v-model="selectedCount"
          class="select"
          v-on:change="$emit('change-count', product.productId , selectedCount)"
      >
        <option
          v-for="n in product.inventory"
        >
          {{n}}
        </option>
      </select>
      <div
          v-on:click="$emit('toggle-alert')"
          v-if="isChooseTry(product.productId) === 'try_active'"
          class="trial_info trial_underline">
        <strong>Trial Price (Limit {{ product.trialLimit }}):</strong>
      </div>
      <div v-if="product.discount > 0" class="trial_info"><strong>Save {{ product.discount }}%:</strong></div>
      <div
          v-if="reward && checkIsChoose(product.productId)"
          class="trial_info reward"
      >
        Reward:
      </div>
    </td>
    <td class="product_top_align">
      <div class="price">
        <span v-if="trialProductId === product.productId || product.discount > 0" class="price_line-through">${{product.price.toFixed(2)}}</span>
        <span v-else class="normal_price"><strong>${{product.price.toFixed(2)}}</strong></span>
        <span v-if="trialProductId === product.productId || product.discount > 0" class="normal_price"><strong>${{ calculatePrice(product.price , product.trialDiscount , product.discount).toFixed(2) }}</strong></span>
        <span v-if="trialProductId === product.productId" class="trial_price">-${{ product.trialDiscount.toFixed(2) || product.discount.toFixed(2) }}</span>
        <span v-if="product.discount > 0" class="trial_price">-${{ (product.price * product.discount / 100).toFixed(2)  }}</span>
        <span
            v-if="reward && checkIsChoose(product.productId)"
            class="trial_price reward"
        >-${{ findCountProduct(product.productId).toFixed(2) }}</span>
      </div>
    </td>
  </tr>
</template>


<script>

  export default {
    props: ['product' , 'trialProductId','setTrial','reward','order'],
    data() {
      return {
        selectedCount: 1
      }
    },
    methods: {
      isChooseTry(id) {
        if(id === this.trialProductId) {
          return 'try_active'
        } else {
          return 'try_not-active'
        }
      },
      calculatePrice(price , trialdiscount , discount) {
        if(trialdiscount) {
          return price - trialdiscount
        } else if (discount) {
          return price - (price * discount / 100)
        } else {
          return price
        }
      },
      checkIsChoose() {
        let result = this.order.findIndex(i => i.productId === this.product.productId)
        if(result === -1) {
          return  false
        } else {
          return true
        }
      },
      findCountProduct(id) {
        let prodInd = this.order.findIndex( i => i.productId === id)
        let count = this.order[prodInd].count
        return count * this.reward
      }
    }
  }

</script>

<style>
  p {
    margin: 0;
  }

  .product_checkbox,
  .product_image {
    text-align: center;
  }

  .product_box input[type=checkbox] {
    visibility: hidden;
  }


  .product_box {
    width: 16px;
    height: 16px;
    position: relative;
    border: 1px solid #CCCCCC;
    border-radius: 2px;
    margin-left: calc( 50% - 8px);
  }

  .product_box label {
    cursor: pointer;
    position: absolute;
    width: 10px;
    height: 10px;
    left: 2px;
    top: 2px;
  }

  .product_box input[type=checkbox]:checked + label{
    background: #458500;
  }

  .ifChecked {
      border-color: #458500;
  }

  .product_top_align {
    vertical-align: top;
  }

  .info {
    display: flex;
    align-items: center;
    margin-right: 10px;
    margin-bottom: 10px;
  }



  .try_active,
  .try_not-active
  {
    padding: 2px 16px 2px 8px;
    text-align: start;
    border: none;
    margin-right: 8px;
    font-size: 12px;
    line-height: 16px;
    position: relative;
    height: 20px;
    width: 52px;
    outline: none;
    cursor: pointer;
    overflow: hidden;
  }

  .try_active {
    background-color: green;
    color: #ffffff;
  }

  .try_not-active {
    background-color: #CCCCCC;
    color: #666666;
  }

  .active_applied {
    font-style: italic;
    font-size: 14px;
    line-height: 20px;
    color: #666666;
    margin-left: 11px;
  }

  .not_active_applied {
    font-style: normal;
    font-size: 14px;
    line-height: 20px;
    color: #458500;
  }

  .product_id {
    font-size: 12px;
    line-height: 16px;
    color: #666666;
    padding-top: 4px;
  }

  .product_description {
    max-width: 280px;
    font-size: 14px;
    line-height: 20px;
    color: #3f3f3f;
  }

  .product_price {
    font-size: 14px;
    line-height: 20px;
    color: #3f3f3f;
    margin: 0 32px 0 28px;
  }

  .trial_info {
    font-size: 12px;
    line-height: 16px;
    color: #458500;
    margin-top: 29px;
    padding-left: 49px;
    display: flex;
    justify-content: flex-end;
  }

  .trial_underline {
    text-decoration: underline;
  }

  .reward {
    margin-top: 7px;
  }

  .price {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .price_line-through {
    text-decoration: line-through;
    font-family: Roboto;
    font-style: normal;
    font-weight: 500;
    font-size: 16px;
    line-height: 24px;
    color: #cccccc;
  }

  .normal_price {
    line-height: 24px;
    color: #3f3f3f;
  }

  .trial_price {
    font-size: 12px;
    line-height: 16px;
    color: #BD3C37;
    width: 100%;
    text-align: right;
  }

  .quest_svg,
  .trial_underline {
    cursor: pointer;
  }

</style>