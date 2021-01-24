<template>
  <tbody>
    <productTableItem
        v-for="(product) in products"
        v-bind:product="product"
        v-bind:trialProductId="trialProductId"
        v-on:choose-trial="chooseTrial"
        v-on:order-choose="orderProduct"
        v-on:change-count="changeProductCount"
        v-bind:reward="reward"
        v-bind:order="order"
        v-on:toggle-alert="toggleAlertMessage"
    />
  </tbody>
</template>

<script>
  import productTableItem from "./productTableItem";

  export default {
    name: 'productsTableList',
    props: ['products','trialProductId','setTrial','reward','order'],
    components: {
      productTableItem
    },
    methods: {
      chooseTrial(id) {
        this.$emit('choose-trial', id)
      },
      orderProduct(id, count) {
        this.$emit('order-choose', id, count)
      },
      changeProductCount(id , count) {
        this.$emit('change-count', id , count)
      },
      toggleAlertMessage() {
        this.$emit('toggle-alert')
      }
    }
  }

</script>