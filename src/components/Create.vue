// Create.vue

<template>
    <div id="addmatch">
        <div class="match">
            <div class="match-header">
                <h3>Add Problem</h3>
            </div>
            <div class="match-body">
                <form v-on:submit.prevent="addMatch">
                    <div class="form-group">
                        <label>Problem name:</label>
                        <input type="text" class="form-control" v-model="item.problemName"/>
                    </div>
                    <div class="form-group">
                        <label>Problem subject:</label>
                        <input type="text" class="form-control" v-model="item.problemSubject"/>
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
                        <input type="submit" class="btn btn-update" value="Add Match"/>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<!--
def is_true(boolean_value):
if boolean_value:
  return True
return False
return true #distractor -->

<script>

    function convertParsonString(parsonString) {

        const resultingObject = []

        parsonString.split("\n").forEach(row => {
            if (row.length === 0) {
                return
            }

            const distractor = row.search("#distractor") !== -1
            if (distractor) {
                row = row.split("#distractor")[0];
            }

            resultingObject.push({
                "rule": row.trim(),
                "ident": row.match(/^\s*/)[0].length,
                "distractor": distractor
            })
        })

        return resultingObject;
    }

    function parseHints(hintsArray) {

        const resultingObject = [
            {
                hintName: "EliminateDistractor",
                hintValid: false
            },
            {
                hintName: "PartlyCorrectExercise",
                hintValid: false
            },
            {
                hintName: "ShowCorrectness",
                hintValid: false
            }
        ]

        for (let hint in hintsArray) {
            for(let checkHint in resultingObject) {
                if(hintsArray[hint] === resultingObject[checkHint].hintName) {
                    resultingObject[checkHint].hintValid = true;
                }
            }
        }

        return resultingObject;
    }

    export default {
        components: {
            name: 'AddMatch'
        },
        data() {
            return {
                item: {
                    problemName: "",
                    problemSubject: "",
                    problemPiecesRaw: "",
                    problemPieces: [],
                    problemHintsRaw: [],
                    problemHints: []
                }
            }
        },
        methods: {
            addMatch() {

                this.item.problemPieces = convertParsonString(this.item.problemPiecesRaw);
                this.item.problemHints = parseHints(this.item.problemHintsRaw);
                delete this.item.problemPiecesRaw;
                delete this.item.problemHintsRaw;
                console.log(this.item);

                let uri = 'http://siskofasa.nl:3000/api/parsonsproblems/';
                this.axios.defaults.headers.post['Access-Control-Allow-Origin'] = '*';
                this.axios.defaults.headers.post['Content-Type'] ='application/json;charset=utf-8';
                    this.axios.post(uri, this.item).then((response) => {
                        console.log(response.data)
                    });
            }
        }
    }
</script>