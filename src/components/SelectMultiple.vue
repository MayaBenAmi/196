<template>
    <div id="select-multiple">
        <div id="bg" :style="{background: bgColor}">
            <div id="instruction">{{ title }}</div>
            <div v-for="(option, index) in options" :key="index" class="answer" :id="'opt' + (index)" @click="selectOpt">
                <img src="../assets/media/checkbox.png" alt="multiple-choice" class="check-box">
                <img src="../assets/media/check.svg" alt="checkmark" class="checkmark" v-if="selectedArr.includes(index)">
                <div class="multiple-choice">{{ option }}</div>
            </div>
            <div id="checkBtn" @click="checkAns">{{ btnText }}</div>
        </div>
    </div>
</template>


<script>
import json from "../../text.json";
export default {
    name: "select-multiple",
    props: ["questionNum"],
    computed: {
        options() {
            return json.selectMultiple[this.questionNum].options;
        },
        title() {
            return json.selectMultiple[this.questionNum].title;
        },
        correctAns() {
            return json.selectMultiple[this.questionNum].correctAns;
        }
    },
    data() {
        return {
            selectedArr : [],
            btnText: "בדוק אותי",
            bgColor: "#fce9a9",
        }
    },
    methods: {
        selectOpt(event) {
            let selectedOption = Number(event.currentTarget.id.charAt(3));
            if (!this.selectedArr.includes(selectedOption)) {
                    if (this.selectedArr.length>= 3) {
                    alert("סמן שלוש אפשרויות בלבד");
                } else {
                    this.selectedArr.push(selectedOption);
                }
            } else {
                let newArr = [];
                for (let i = 0; i < this.selectedArr.length; i++) {
                    if (this.selectedArr[i] !== selectedOption) {
                        newArr.push(this.selectedArr[i]);
                    }
                }
                this.selectedArr = newArr;
            }
        },
        checkAns() {
            const sortedArr1 = this.selectedArr.sort();
            const sortedArr2 = this.correctAns.sort();
            if (this.btnText === "בדוק אותי") {
                if (JSON.stringify(sortedArr1) !== JSON.stringify(sortedArr2)) {
                    this.btnText = "תנסה שוב";
                    this.bgColor = "rgb(247, 83, 83)";
                } else {
                    this.btnText = "מטורףף";
                    this.bgColor = "rgb(179, 241, 160)";
                }
            } else {
                this.btnText = "בדוק אותי";
                this.bgColor = "#fce9a9";
                this.selectedArr = [];
            }
        }

    }
}
</script>

<style scoped>
#select-multiple {
    margin: 0;
    height: fit-content;
    margin-bottom: 10%;
    width: 100vw;
    position: relative;
    top: 0;
    right: 0;
}
#bg {
    list-style-type: none;
    padding: 1%;
    margin: auto;
    margin-top: 10%;
    margin-bottom: 0px;
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
.answer {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    margin-bottom: 1.5vh;
    justify-content: start;
    align-items: center;
}
.check-box {
    height: 5vh;
    margin-left: 3vw;
    margin-right: 3%;
}
.multiple-choice {
    background-color: #ffc300;
    margin: 1%;
    padding: 1vh;
    border-radius: 1vh;
    font-size: 2.5vh;
    text-align: start;
    color: #001d3d;
}


.checkmark {
    height: 6vh;
    position: absolute;
    margin-right: 0%;
    margin-bottom: 2%;
    display: block;
}
#checkBtn {
    margin-top: 6%;
    margin-right: 1%;
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