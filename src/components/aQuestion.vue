<template>
    <section class="section">
        <div class="container">
            <h1 class="title is-4">{{question.def}}</h1>
            <form @submit="answerQuestion" class="form">
                <div v-for="answer in answers" v-bind:key="answer.id">
                    <b-field>
                        <b-radio-button v-model="qAnswer"
                            :native-value="answer.a"
                            type="is-info">
                            {{answer.a}}
                        </b-radio-button>
                    </b-field>
                </div>
                <b-button v-on:click="answerQuestion" class="questionButton" type="is-success">Submit</b-button>
                <b-button v-on:click="$emit('next', 0)" class="skip" type="is-warning" outlined>Skip</b-button>
            </form>
        </div>
    </section>
</template>

<script>
export default {
    name: "Question",
    props: {
        question: {type: Object, required: true, },
        answers: {type: Array, required: true, },
        correct: {type: String, required: true, }
    },
    data(){
        return{
            qAnswer:""
        }
    },
    methods:{
        answerQuestion(e){
            e.preventDefault();
            if (this.qAnswer.toLowerCase() === this.correct.toLowerCase()){
                this.$emit('next', 1);
                this.qAnswer = "";
            }
            else{
                //alert(this.question.word)
                this.$buefy.dialog.alert({
                    title: 'Incorrect',
                    message: this.question.word + " : " + this.question.def,
                    type: "is-dark",
                    confirmText: 'Ok'
                })
                this.$emit('next', 0);
                this.qAnswer = "";
            }
        }
    }
}
</script>

<style scoped>


</style>