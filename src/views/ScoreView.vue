<template>
  <div id="container">
    <p id="score_p">{{ score }}</p>
    <form>
      <Default-Input id='user-name'
                     class="m-3"
                     placeholder="Your Name"
                     v-model="userName"
                     :readonly='isScoreAndNameSubmitted'
                     />
      <Default-Button class="m-3"
                      value="Submit"
                      @click.native="submitNameAndScore"
                      />
    </form>

    <Default-Table :bodyContent="scoreRecords"/>

    <router-link to="/game-field">
      <Default-Button class="m-3" value="Play again"/>
    </router-link>

    <p style='margin: 7px'>or</p>

    <router-link to="/learning-mode">
      <Default-Button value="Learn verbs"/>
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
        scoreRecords: null,

        userName: "",
        isScoreAndNameSubmitted: false,

        score_DOM_element: null,
        userName_DOM_element: null
      }
    },

    props: {
      score: {
        type: Number,
        required: true
      },
      database: {
        type: Object,
        required: true
      }
    },

    components: {
      DefaultInput,
      DefaultButton,
      DefaultTable
    },

    methods: {
      serveScoreRecords() {
        let scoreRecords = [];

        this.database.ref('scores').orderByChild('score').once("value")
          .then(snapshot => {
            // forEach() needed here because of the way JavaScript objects work,
            // the ordering of data in the JavaScript object returned by val()
            // is not guaranteed to match the ordering on the server nor the
            // ordering of child_added events. That is where forEach() comes
            // in handy. It guarantees the children of a DataSnapshot will be
            // iterated in their query order.
            snapshot.forEach(childSnapshot => {
              scoreRecords.push(childSnapshot.val());
            });
          })
          .then(() => {
            scoreRecords.reverse();
            scoreRecords = this.filterTableByName(scoreRecords.slice());
            scoreRecords = this.formatDates(scoreRecords.slice());
            this.scoreRecords = scoreRecords;
          });
      },

      filterTableByName(scoreRecords) {
        let filteredScoreTable = [];
        let names = {};

        for (let row of scoreRecords) {
          if (names.hasOwnProperty( row.name )) continue;

          filteredScoreTable.push( row );
          names[ row.name ] = null;
        }

        return filteredScoreTable;
      },

      formatDates(scoreRecords) {
        let date, day, monthIndex, year;

        let monthNames = [
          "Jan", "Feb", "Mar",
          "Apr", "May", "Jun", "Jul",
          "Aug", "Sep", "Oct",
          "Nov", "Dec"
        ];

        for (let row of scoreRecords) {
          date = new Date(row.date);

          day = date.getDate();
          monthIndex = date.getMonth();
          year = date.getFullYear();

          row.date = `${day} ${monthNames[monthIndex]} ${year}`;
        }

        return scoreRecords;
      },

      async submitNameAndScore() {
        if (this.isScoreAndNameSubmitted) {
          alert('You have already submitted your score.');
          return;
        }

        if (this.score === 0) {
          this.createAndShowTippy(this.score_DOM_element,
            'Score is zero. Come on, is that all you got? ;)', 'top');
          return;
        }

        if (this.userName.trim() === "") {
          this.createAndShowTippy(this.userName_DOM_element,
            'Name field is empty. What is your name?', 'top');
          return;
        }

        let scoreRecord = {
          name: this.userName,
          score: this.score,
          date: Date.now()
        };

        this.database.ref('scores').push(scoreRecord);

        this.serveScoreRecords();

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
      this.serveScoreRecords();
    },

    mounted() {
      tippy.setDefaults({
        trigger: 'mouseenter',
        arrow: true,
        interactive: true,
        theme: 'light-border',
        ignoreAttributes: true
      });

      this.score_DOM_element = document.getElementById('score_p');
      this.userName_DOM_element = document.getElementById('user-name');
    },

    watch: {
      userName: function () {
        if (this.userName_DOM_element._tippy)
          this.userName_DOM_element._tippy.destroy();
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