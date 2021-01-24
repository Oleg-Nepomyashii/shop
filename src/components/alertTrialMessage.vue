<template>
  <div v-show="isShowAlert" class="container">
    <div class="alert">

    </div>
    <div class="msg_container">
      <div class="msg_inner">
        <h2 class="alert_title">
          Try it!
        </h2>
        <p class="alert_message">
          Limited to one trial item per customer per order. If more than one trial item is added, trial pricing will automatically apply to the first item added to cart only. Limited to one-time use per product and limited to first-time purchase of product only. May not be combined with other offers.
        </p>
        <button
            class="alert_btn"
            v-on:click="$emit('toggle-alert')"
        >
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M17 1L1 17M17 17L1 1.00002" stroke="#3F3F3F"/>
          </svg>
        </button>
      </div>
    </div>
  </div>

</template>

<script>
  export default {
    props: ['isShowAlert']
  }
</script>

<style scoped>

  .container {
    position: relative;
  }

  .alert {
    z-index: 1;
    position: absolute;
    height: 100vh;
    width: 100vw;
    background: #000000;
    opacity: 0.7;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .msg_inner {
    background: #ffffff;
    padding: 24px;
    max-width: 360px;
    position: relative;
  }

  .msg_container {
    position: absolute;
    z-index: 2;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .alert_title {
    font-size: 24px;
    line-height: 32px;
    color: #3f3f3f;
    margin: 0;
  }

  .alert_message {
    margin: 8px 0 0;
    font-size: 14px;
    line-height: 20px;
    color: #3f3f3f;
  }

  .alert_btn {
    position: absolute;
    top: 20px;
    right: 20px;
    background: transparent;
    padding: 0;
    border: none;
    cursor: pointer;
  }

</style>