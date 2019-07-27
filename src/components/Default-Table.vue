<template>
  <table id='score_table'>
    <thead>
    <tr>
      <th>#</th>
      <th>Name</th>
      <th>Score</th>
      <th>Date</th>
    </tr>
    </thead>
    <tbody id='score_table_body'>
    <tr>
      <td>1</td>
      <td>testName1</td>
      <td>Score1</td>
      <td>Date1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>testName2</td>
      <td>Score2</td>
      <td>Date2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>testName3</td>
      <td>Score3</td>
      <td>Date3</td>
    </tr>
    </tbody>
  </table>
</template>

<script>
  export default {
    name: "Default-Table"
  }
</script>

<style scoped>
  table {
    box-shadow: 0 1px 3px 0 rgba(0,0,0,0.19);
    margin: 5px;
  }

  #score_table {
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