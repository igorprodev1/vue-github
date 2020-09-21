<template>
  <table class="data-table">
    <thead>
      <tr>
        <th v-for="header in headers" :key="header.prop">{{header.label}}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="row in dataSourse" :key="row.id">
        <td v-for="header in headers" :key="header.prop">{{row[header.prop]}}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    dataSourse: {
      type: Array,
      default: () => [],
    },
    headers: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  border-collapse: collapse;
  width: 100%;
  height: 100%;
}
</style>
