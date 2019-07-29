<template>
  <table>
    <thead>
      <tr>
        <th>#</th>
        <th>Name</th>
        <th>Score</th>
        <th>Date</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(row, index) in bodyContent" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ row.name }}</td>
        <td>{{ row.score }}</td>
        <td>{{ row.date }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
  export default {
    name: "Default-Table",
    props: {
      bodyContent: Array
    }
  }
</script>

<style scoped>
  table {
    box-shadow: 0 1px 3px 0 rgba(0,0,0,0.19);
    margin: 5px;
    padding-top: 5px;
    padding-bottom: 5px;
    padding-left: 5px;
    display: block;
    overflow-y: auto;
    max-height: 244px;
  }

  td, th {
    padding-right: 10px;
  }
</style>