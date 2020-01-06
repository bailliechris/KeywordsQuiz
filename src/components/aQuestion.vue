<template>
    <section class="section">
        <div class="container">
            <h1 class="title is-6">{{question.def}}</h1>
            <form @submit="answerQuestion" class="form">
                <b-field>
                    <b-input type="text" class="answerBox" placeholder="Answer..." v-model="qAnswer"></b-input>
                </b-field>
            </form>
            <b-button v-on:click="answerQuestion" class="questionButton" type="is-success">Submit</b-button>
            <b-button v-on:click="$emit('next', 0)" class="skip" type="is-warning" outlined>Skip</b-button>
        </div>
    </section>
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
                //alert(this.question.word)
                this.$buefy.dialog.alert({
                    title: 'Incorrect',
                    message: this.question.word,
                    confirmText: 'Ok'
                })
                this.$emit('next', -1);
                this.qAnswer = "";
            }
        }
    }
}
</script>

<style scoped>


</style>