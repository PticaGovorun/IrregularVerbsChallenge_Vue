<template>
  <div id="container">
    <p id="score_p" class='display-2'>{{ score }}</p>
    <v-form ref='form' class='d-flex'>
      <v-text-field id='user-name'
                    label="Your Name"
                    v-model="userName.value"
                    :readonly='isScoreAndNameSubmitted'
                    :rules='userName.rules'
                    required
                    outlined
                    ></v-text-field>
      <v-btn outlined
             type='submit'
             @click.native="submitNameAndScore"
             class='ml-3'
             height='56'
             >Submit</v-btn>
    </v-form>

    <Default-Table :bodyContent="scoreRecords"
                   :highlightedRow='lastRecordIndex'/>

    <v-btn class='mt-3' outlined to='/game-field'>Play again</v-btn>
    <p class='ma-1'>or</p>
    <v-btn outlined to='/learning-mode'>Learn verbs</v-btn>
  </div>
</template>

<script>
  import DefaultTable from "@/components/Default-Table.vue";

  import tippy from 'tippy.js';
  import 'tippy.js/themes/light-border.css';

  export default {
    name: "ScoreView",

    data: function () {
      return {
        scoreRecords: null,

        userName: {
          value: '',
          rules: [val => (val || '').length > 0 || 'What is your name?']
        },

        isScoreAndNameSubmitted: false,

        score_DOM_element: null,

        lastRecordIndex: null
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
      DefaultTable
    },

    methods: {
      serveScoreRecords() {
        let scoreRecords = [];

         return this.database.ref('scores').orderByChild('score').once("value")
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

      submitNameAndScore() {
        if (this.isScoreAndNameSubmitted) {
          alert('You have already submitted your score.');
          return;
        }

        if (this.score === 0) {
          this.createAndShowTippy(this.score_DOM_element,
            'Score is zero. Come on, is that all you got? ;)', 'top');
          return;
        }

        if (!this.$refs.form.validate()) return;

        let newScoreRecord = {
          name: this.userName,
          score: this.score,
          date: Date.now()
        };

        this.database.ref('scores').push(newScoreRecord);
        this.isScoreAndNameSubmitted = true;

        this.serveScoreRecords()
          .then(() => this.lastRecordIndex =
            this.findLastRecordIndex(newScoreRecord));
      },

      findLastRecordIndex(newScoreRecord) {
        for (let [index, record] of this.scoreRecords.entries()) {
          if (record.name === newScoreRecord.name)
            return index;
        }
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
</style>