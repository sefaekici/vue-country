<template>
  <div class="home">
    <country-table></country-table>
  </div>
</template>

<script>
import Table from "../components/CountryTable.vue"
export default {
  name: 'Home',
  components: {
    "country-table":Table
  }
}
</script>
