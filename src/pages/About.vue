<template>
  <q-page padding class="column q-gutter-md flex-center">
    <!-- content -->
    <div class="text-center text-subtitle1">Frank Albarracín</div>
    <div class="text-center text-subtitle1">
      <q-icon name="fab fa-twitter-square" class="text-primary" /> @efrankme
    </div>
    <div class="text-center text-subtitle1">
      <!-- <q-icon name="fab fa-paypal" class="text-primary" /> -->
      <a
        class="text-teal"
        target="_blank"
        href="https://paypal.me/frankdroid"
        >paypal.me/frankdroid</a
      >
      <br>
      BTC
      <q-badge
        outline
        color="teal"
        label="14mi9yrPabUUPx6ZE9i4fiLXZ4Nsfmy4nv"
      /><br />
      USDT
      <q-badge
        outline
        color="teal"
        label="0xba60a9e61aedb278bc6d3ce8e8db8c8e441f6069"
      /><br />
    </div>
    <q-btn to="/" label="Volver" color="primary" />
  </q-page>
</template>

<script>
export default {
  name: "About"
};
</script>
