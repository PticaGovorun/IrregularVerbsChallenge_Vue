<template>
  <div id="container">
    <p id="score_p">{{ score }}</p>
    <form>
      <Default-Input class="m-3" placeholder="Your Name"/>
      <Default-Button class="m-3" value="Submit"/>
    </form>
    <Default-Table :bodyContent="scoreTable"/>
    <router-link
        to="/GameField"><Default-Button class="m-3" value="Back"/></router-link>
  </div>
</template>

<script>
  import DefaultInput from "@/components/Default-Input.vue";
  import DefaultButton from "@/components/Default-Button.vue";
  import DefaultTable from "@/components/Default-Table.vue";

  export default {
    name: "ScoreView",

    data: function () {
      return {
        scoreTable: null
      }
    },

    props: {
      score: {
        type: Number,
        required: true
      }
    },

    components: {
      DefaultInput,
      DefaultButton,
      DefaultTable
    },

    methods: {
      async serveScoreTable() {
        let response = await fetch("json_score_tabl_db_get.php");
        this.scoreTable = await response.json(); // returns Array

        this.scoreTable = this.filterTableByName(this.scoreTable.slice());
        this.scoreTable = this.formatDates(this.scoreTable.slice());
      },

      filterTableByName(scoreTable) {
        let filteredScoreTable = [];
        let names = {};

        for (let row of scoreTable) {
          if (names.hasOwnProperty( row.name )) continue;

          filteredScoreTable.push( row );
          names[ row.name ] = null;
        }

        return filteredScoreTable;
      },

      formatDates(scoreTable) {
        let date, day, monthIndex, year;

        let monthNames = [
          "Jan", "Feb", "Mar",
          "Apr", "May", "Jun", "Jul",
          "Aug", "Sep", "Oct",
          "Nov", "Dec"
        ];

        for (let row of scoreTable) {
          date = new Date(row.date);

          day = date.getDate();
          monthIndex = date.getMonth();
          year = date.getFullYear();

          row.date = `${day} ${monthNames[monthIndex]} ${year}`;
        }

        return scoreTable;
      }
    },
    created() {
      this.serveScoreTable();
    }
  }
</script>

<style scoped>
  #container {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    text-align: center;
    margin: 0 40px 0 40px;
  }

  #score_p {
    font-size: 250%;
    margin: 10px;
  }

  form {
    display: flex;
  }

  .m-3 {
    margin: 3px;
  }
</style>