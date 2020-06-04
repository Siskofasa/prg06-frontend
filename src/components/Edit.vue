// Edit.vue

<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                <h3>Edit Match</h3>
            </div>
            <div class="card-body">
                <form v-on:submit.prevent="updateItem">
                    <div class="form-group">
                        <label>Problem name:</label>
                        <input type="text" class="form-control" v-model="item.problemName"/>
                    </div>
                    <div class="form-group">
                        <label>Problem subject:</label>
                        <input type="text" class="form-control" v-model="item.problemSubject" />
                    </div>
                    <div class="form-group">
                        <label>Problem code:</label>
                        <textarea v-model="item.problemPiecesRaw"></textarea>
                    </div>
                    <div class="form-group">
                        <label>Hints:</label>
                        <input type="checkbox" id="distractorEliminateHint" value="EliminateDistractor"
                               v-model="item.problemHintsRaw">
                        <label for="distractorEliminateHint">Eliminate Distractor</label>
                        <input type="checkbox" id="partlyCorrectExerciseHint" value="PartlyCorrectExercise"
                               v-model="item.problemHintsRaw">
                        <label for="partlyCorrectExerciseHint">Partly Correct Exercise</label>
                        <input type="checkbox" id="showCorrectnessHint" value="ShowCorrectness"
                               v-model="item.problemHintsRaw">
                        <label for="showCorrectnessHint">Show Correctness</label>
                        <!--                        <input type="text" class="form-control" v-model="item.problemHints"/>-->
                    </div>
                    <div class="form-group">
                        <input type="submit" class="btn btn-primary" value="Update Item"/>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return{
                item:{}
            }
        },

        created: function(){
            this.getItem();
        },

        methods: {
            getItem()
            {
                let uri = 'http://siskofasa.nl:3000/api/parsonsproblems/edit/' + this.$route.params.id;
                this.axios.get(uri).then((response) => {
                    this.item = response.data;
                });
            },

            updateItem()
            {
                let uri = 'http://siskofasa.nl:3000/api/parsonsproblems/update/' + this.$route.params.id;

                console.log("Edit.vue");

                this.axios.post(uri, this.item).then((response) => {
                    this.$router.push({name: 'Index'});
                });
            }
        }
    }
</script>