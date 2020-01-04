<template>
    <div>
        <h1 class="questionText">{{question.def}}</h1>
        <form @submit="answerQuestion" class="form">
            <input type="text" class="answerBox" placeholder="Answer..." v-model="qAnswer">
            <input type="button" v-on:click="answerQuestion" class="questionButton" value="Submit">
            <input type="button" v-on:click="$emit('next', 0)" class="skip" value="Skip">
        </form>
    </div>
</template>

<script>
export default {
    name: "Question",
    props: ["question"],
    data(){
        return{
            qAnswer:""
        }
    },
    methods:{
        answerQuestion(e){
            e.preventDefault();
            if (this.qAnswer.toLowerCase() === this.question.word.toLowerCase()){
                this.$emit('next', 1);
                this.qAnswer = "";
            }
            else{
                alert(this.question.word)
                this.$emit('next', -1);
                this.qAnswer = "";
            }
        }
    }
}
</script>

<style scoped>
.questionButton{
    display:flex;
    background:cornsilk;
    padding: 10px;
    border-bottom: 1px #ccc dotted;
    align-items: center;
    justify-content: space-around;
    order: 4;
    max-width: 10%;
    flex: 1;
}

.form{
    display:flex;
    align-items: center;
    flex-wrap: wrap;
    justify-content: space-evenly;
}

.skip {
    display:flex;
    background: red;
    color:white;
    padding: 10px;
    border-radius: 50%;
    justify-content: space-around;
    flex-basis:5%;
    cursor: pointer;
    flex:1;
    order: 1;
    max-width:10%;
}
.questionText{
    color:darkslategray;
    font-size: 1rem;
}

.answerBox{
    display:flex;
    background-color: antiquewhite;
    color: darkslategray;
    border-bottom: 1px darkslategrey;
    font-size: 1rem;
    padding:10px;
    justify-content: space-around;
    text-align: center;
    flex:10;
    max-width: 50%;
    order: 3;
}
</style>