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
      <tr v-for="(row, index) in bodyContent"
          :key="index"
          :class='{highlight: index === highlightedRow}'
          >
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
      bodyContent: Array,
      highlightedRow: Number
    }
  }
</script>

<style scoped>
  table {
    box-shadow: 0 1px 3px 0 rgba(0,0,0,0.19);
    margin: 5px;
    padding-top: 5px;
    padding-bottom: 5px;
    display: block;
    overflow-y: auto;
    max-height: 244px;
    border-collapse: collapse;
  }

  td {
    padding: 4px 10px 4px 10px;
  }

  .highlight {
    background-color: #eed9a3;
  }
</style>