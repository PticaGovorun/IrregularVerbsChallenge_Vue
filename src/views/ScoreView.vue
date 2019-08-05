<template>
  <div id="container">
    <p id="score_p">{{ score }}</p>
    <form>
      <Default-Input id='user-name'
                     class="m-3"
                     placeholder="Your Name"
                     v-model="userName"/>
      <Default-Button class="m-3"
                      value="Submit"
                      @click.native="submitNameAndScore"/>
    </form>

    <Default-Table :bodyContent="scoreTable"/>

    <router-link to="/GameField">
      <Default-Button class="m-3" value="Back"/>
    </router-link>
  </div>
</template>

<script>
  import DefaultInput from "@/components/Default-Input.vue";
  import DefaultButton from "@/components/Default-Button.vue";
  import DefaultTable from "@/components/Default-Table.vue";

  import tippy from 'tippy.js';
  import 'tippy.js/themes/light-border.css';

  export default {
    name: "ScoreView",

    data: function () {
      return {
        scoreTable: null,
        userName: "",
        isScoreAndNameSubmitted: false,

        scoreDOM: null,
        userNameDOM: null
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
      },

      async submitNameAndScore() {
        if (this.isScoreAndNameSubmitted) {
          alert('You have already submitted your record. Press "Back" and play again.');
          return;
        }

        if (this.score === 0) {
          this.createAndShowTippy(this.scoreDOM,
            'Score is zero. Come on, is that all you got? ;)', 'top');
          return;
        }

        if (this.userName.trim() === "") {
          this.createAndShowTippy(this.userNameDOM,
            'Name field is empty. What is your name?', 'top');
          return;
        }

        let nameAndScore = {
          "name": this.userName,
          "score": this.score
        };

        let response = await fetch("json_score_tabl_db_post.php", {
          method: 'POST',
          headers: {'Content-Type': "application/x-www-form-urlencoded"},
          body: "dbParams_json=" + JSON.stringify(nameAndScore)
        });
        this.scoreTable = await response.json();

        this.scoreTable = this.filterTableByName(this.scoreTable.slice());
        this.scoreTable = this.formatDates(this.scoreTable.slice());

        this.userName = '';
        this.isScoreAndNameSubmitted = true;
      },

      createAndShowTippy(target, content, placement) {
        tippy(target, {
          content: content,
          placement: placement
        });

        target._tippy.show();
      }
    },

    created() {
      this.serveScoreTable();
    },

    mounted() {
      tippy.setDefaults({
        trigger: 'mouseenter',
        arrow: true,
        interactive: true,
        theme: 'light-border',
        ignoreAttributes: true
      });

      this.scoreDOM = document.getElementById('score_p');
      this.userNameDOM = document.getElementById('user-name');
    },

    watch: {
      userName: function () {
        this.userNameDOM._tippy.destroy();
      }
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