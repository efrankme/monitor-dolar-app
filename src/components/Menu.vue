<template>
  <q-list >
    <q-item-label header></q-item-label>
    <q-item clickable tag="a" to="/" class="text-blue-grey-8">
      <q-item-section avatar>
        <q-icon name="web_asset" />
      </q-item-section>

      <q-item-section>
        <q-item-label>Dolar Promedio</q-item-label>
      </q-item-section>
    </q-item>

    <q-item clickable tag="a" to="/about" class="text-blue-grey-8">
      <q-item-section avatar>
        <q-icon name="account_box" />
      </q-item-section>

      <q-item-section>
        <q-item-label>Acerca</q-item-label>
      </q-item-section>
    </q-item>
  </q-list>
</template>

<script>
export default {
  name: 'Menu'
}
</script>
