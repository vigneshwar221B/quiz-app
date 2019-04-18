<template>
    <div>

        <b-jumbotron >

            <template slot="lead">
             {{ question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item :class="showUI(answ)" @click="selectAnswer(answ)" v-for = "(answ,ind) in answers" :key="ind">{{ answ }}</b-list-group-item>
            </b-list-group>
         
            <b-button class = "btns" :disabled="!isSelect || isSubmitted>0" variant="primary" @click = "checkAnswer">submit</b-button>
            <b-button class = "btns" :disabled="quesnum>10" @click="nextUI" variant="success" href="#">next</b-button>

        </b-jumbotron>
        <p v-if = "quesnum>10">
            Hurray! you finished the test
        </p>
    </div>
    
</template>

<script>
import _ from 'lodash';
export default {
    props: {
        question: String,
        next: Function,
        canswer:String,
        answers: Array,
        quesnum: Number,
        updateScore: Function
    },
    data(){
        return {
            isSelect : false,
            isCorrect : false,
            isSubmitted : 0,
            score: 0, 
            selectedVal : ''
        }
    },
    mounted(){
        this.shuffleAnswers()
        this.isCorrect = false
        this.isSelect = false
        this.isSubmitted = 0
        this.score = 0
    },
    methods:{
        nextUI(){
            this.isSelect = false
            this.isSubmitted = 0
            this.next()
        },
        showUI(value){
            let classname = "";
            if(this.isSelect && this.isSubmitted==0 && value && this.selectedVal == value){
                classname = "selected"
            }else if(this.isSelect && this.isSubmitted>0 && value !== this.canswer){
                classname = "incorrect"
            }else if(this.isSelect && this.isSubmitted>0 && value === this.canswer){
                classname = "correct"
            }
            return classname;
        },
        selectAnswer(answ){

            this.isSelect = true
            this.selectedVal = answ
            
            if(answ === this.canswer){
                
                this.isCorrect = true;
                
                
            }else{
                
                this.isCorrect = false;
                
            }
        },
        checkAnswer(){
            this.isSubmitted += 1
            if(this.isCorrect){
                this.score += 1;
                
            }else{
                
            }
            this.updateScore(this.score);
            this.showUI()

        },
        
        
        shuffleAnswers(){
            _.shuffle(this.answers)
        }
    }
}
</script>

<style scoped>
    .list-group {
        padding: 10px;
    }
    .list-group-item:hover {
        background: #EEE;
        cursor: pointer;
    }
    .btns {
        margin: 0 10px;
    }
    .selected {background-color: lightblue}

    .correct {background-color: lightgreen}

    .incorrect {background-color: red}

</style>

