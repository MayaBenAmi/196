<template>
    <div id="match">
        <div id="table">
          <div id="instruction">{{ title }}</div>
          <table class="inner-table" :style="{background: bgColor}">
            <tr v-for="(square, index) in column"  :key="index" :class="index === column.length-1 ? '' : 'row'">
              <td class="drop-square">
                <!-- <div class="drop-circle"></div> -->
                <draggable class="list-group" :list="dropOptions[index]" :group="{name:'people', put:true, pull: true}" @end="updateDropOptions(index)">
                    <div v-for="option in dropOptions[index]" class="drag-option" v-if="option!== '[]'"> {{ option }} </div>
                </draggable>
              </td>

                             
              <td class="text-square">{{ square }}</td>
            </tr>
          </table>
          <div class="drag-options" id="drag-options">
            <draggable :list="dragOptions" :group="{ name: 'people', pull: true, put: true}" >
              <div v-for="(option, index) in dragOptions" class="drag-option" :key="index">{{ option }}</div>
            </draggable>
          </div>
          <div id="checkBtn" @click="checkAns">{{ btnText }}</div>
        </div>
    </div>
</template>

<script>
import json from "../../text.json";
import draggable from 'vuedraggable';
  export default {
    name: "match-table",
    props: ["questionNum"],
    components: {
      draggable
    },
    data() {
        return {
          enabled: true,
          dragOptions: json.matching[this.questionNum].dragOptions,
          dropOptions: json.matching[this.questionNum].dropOptions,
          btnText: "בדוק אותי",
          bgColor: "#ffc300"
        }
    },
    methods: {
      checkAns() {
        let correctAnswers = json.matching[this.questionNum].correctAnswers;
        let userAnswers = this.dropOptions;
        
        if (this.btnText === "בדוק אותי") {
          let isCorrect = true;

          // Compare each dropOption against the correctAnswer
          for (let i = 0; i < correctAnswers.length; i++) {
            if (JSON.stringify(userAnswers[i]) !== JSON.stringify(correctAnswers[i])) {
              isCorrect = false;
              break;
            }
          }

          if (isCorrect) {
            this.btnText = "אתה מטורף!";
            this.bgColor = "rgb(179, 241, 160)";
          } else {
            this.btnText = "הממ לא בדיוק...";
            this.bgColor = "rgb(247, 83, 83)";
          }
        } else {
          this.bgColor = "#ffc300";
          this.btnText = "בדוק אותי";
        }
      },
      updateDropOptions(index) {
        // Update dropOptions when an item is dropped
        // You can perform additional checks here if necessary
        this.$set(this.dropOptions, index, this.dropOptions[index]);
      }

    },
    computed: {
      title() {
        return json.matching[this.questionNum].title;
      },
      column() {
        return json.matching[this.questionNum].column;
      },
      // dragOptions() {
      //   return json.matching[this.questionNum].dragOptions;
      // }
    }
  }

</script>

<style scoped>
@font-face {
    font-family: "aduma";
    src: url("../assets/fonts/Aduma Regular.ttf");
  }
  
  #match {
    margin: 0;
    height: fit-content;
    width: 100vw;
    position: relative;
    top: 0;
    right: 0;
  }
  #table {
    list-style-type: none;
    padding: 1%;
    margin: auto;
    margin-top: 0%;
    background-color: #fce9a9;
    border-radius: 1vh;
    width: 95vw;
  }
  #instruction {
    color: #001d3d;
    font-size: 8vw;
    margin: 2%;
    font-weight: 560;
    font-family: "aduma";
  }
  .inner-table {
    border-radius: 2vh;
    margin: auto;
    width: 95%;
    margin-bottom: 2%;
    /* border: #001d3d 0.5vw solid; */
    border-collapse: collapse;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  }
  .row {
    border-bottom:#001d3d solid 0.5vh;
  }
  .drop-square {
    width: 40%;

    border-left: #001d3d solid 0.5vh;
  }
  .list-group {
    display: flex;
    flex-wrap: wrap;
  }
  /* .drop-circle {
    border: #001d3d 0.4vh dashed;
    border-radius: 1vh;
    width: 90%;
    margin: auto;
    margin-top: 5%;
    margin-bottom: 5%;
    height: 90%;
    background-color: #ffc300;
  } */
  .text-square {
    height: fit-content;
    white-space: break-spaces;
    color: #001d3d;
    font-size: 5vw;
    padding: 3.5%;
  }
  .drag-option:active {
    z-index: 1000;
  }
  .drag-options {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    margin-top: 5%;
    justify-content: space-evenly;
    border: #001d3d 0.4vh dashed;
    border-radius: 5%;
    padding-top: 1%;
    margin-bottom: 2vh;
    min-height: 6vh;
    /* bottom: 0%; */
  }

.drag-option {
    border-radius:1vh;
    background-color: #001d3d;
    color: #ffc300;
    padding: 3%;
    font-size: 2.1vh;
    font-weight: bolder;
    margin: 1%;
    /* box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px; */
  }
  #checkBtn {
    margin: 2%;
    background-color: #001d3d;
    font-family: "aduma";
    color: #ffc300;
    font-size: 8vw;
    border-radius: 1.5vh;
    font-weight: 580;
    width: 35vw;
    text-align: center;
    padding: 2%;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  } 
</style>