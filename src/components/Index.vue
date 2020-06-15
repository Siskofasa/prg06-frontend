// Index.vue

<template>
    <div id="overview">
        <div class="title">
            <h1>Archery Matches</h1>
        </div>

        <div class="tiles">
            <div v-for="item in items" :key="item._id">
                <p> {{item.problemName}}</p>
                <b-collapse :id="'collapse-'+item._id" class="mt-2">

                    <form v-on:submit.prevent="updateItem(item)">
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
                            <textarea v-model="item.problemPieces"></textarea>
                        </div>
                        <div class="form-group">
                            <label>Hints:</label>
                            <input type="checkbox" id="distractorEliminateHint" value="EliminateDistractor"
                                   v-model="item.problemHints">
                            <label for="distractorEliminateHint">Eliminate Distractor</label>
                            <input type="checkbox" id="partlyCorrectExerciseHint" value="PartlyCorrectExercise"
                                   v-model="item.problemHints">
                            <label for="partlyCorrectExerciseHint">Partly Correct Exercise</label>
                            <input type="checkbox" id="showCorrectnessHint" value="ShowCorrectness"
                                   v-model="item.problemHints">
                            <label for="showCorrectnessHint">Show Correctness</label>
                            <!--                        <input type="text" class="form-control" v-model="item.problemHints"/>-->
                        </div>
                        <div class="form-group">
                            <input type="submit" class="btn btn-primary btn-update" value="Update Item"/>
                        </div>
                    </form>

                    <!--                <button><router-link :to="{name: 'Edit', params: { id: item._id }}" class="btn btn-primary">Edit</router-link></button>-->
                    <button class="btn btn-danger" v-on:click="deleteItem(item)">Delete</button>
                </b-collapse>
                <b-button v-b-toggle="'collapse-' + item._id" variant="primary" size="sm">Details</b-button>
            </div>
        </div>

    </div>
</template>

<script>

    function buildParsonString(parsonObject) {
        let parsonString = ""

        parsonObject.forEach(parsonRule => {
            // console.log(parsonRule);
            parsonString += `${' '.repeat(parsonRule["ident"])}${parsonRule["rule"]} ${parsonRule["distractor"] ? " #distractor" : ""}\n`
        })
        // console.log(parsonString);
        return parsonString
    }

    function buildHintsString(hintsObject) {
        let hintsString = "";

        hintsObject.forEach(hint => {
            hintsString += `${hint.hintName}: ${hint.hintValid},\n`;
        });

        return hintsString;
    }

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
            for (let checkHint in resultingObject) {
                if (hintsArray[hint] === resultingObject[checkHint].hintName) {
                    resultingObject[checkHint].hintValid = true;
                }
            }
        }

        return resultingObject;
    }

    export default {
        data() {
            return {
                items: []
            }
        },

        created: function () {
            this.fetchItems();
        },

        methods: {
            fetchItems() {
                let uri = 'http://siskofasa.nl:3000/api/parsonsproblems';
                this.axios.get(uri).then((response) => {
                    this.items = response.data.items;
                    for (let item in this.items) {
                        this.items[item].problemPieces = buildParsonString(this.items[item].problemPieces);
                        // this.items[item].problemHints = buildHintsString(this.items[item].problemHints);
                    }
                });
            },

            deleteItem(item) {
                console.log("hallo?");
                let uri = 'http://siskofasa.nl:3000/api/parsonsproblems/' + item._id;
                this.items.splice(this.items.indexOf(item), 1);

                this.axios.delete(uri);
            },

            updateItem(item) {
                let uri = 'http://siskofasa.nl:3000/api/parsonsproblems/' + item._id;

                item.problemPieces = convertParsonString(item.problemPieces);
                item.problemHints = parseHints(item.problemHints);

                console.log(item);

                this.axios.put(uri, item, {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                }).then((response) => {
                    console.log(response.data)
                });
            }
        }
    }
</script>


