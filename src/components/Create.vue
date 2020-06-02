// Create.vue

<template>
    <div id="addmatch">
        <div class="match">
            <div class="match-header">
                <h3>Add Match</h3>
            </div>
            <div class="match-body">
                <form v-on:submit.prevent="addMatch">
                    <div class="form-group">
                        <label>Match name:</label>
                        <input type="text" class="form-control" v-model="item.matchName"/>
                    </div>
                    <div class="form-group">
                        <label>Match date:</label>
                        <input type="text" class="form-control" v-model="item.matchDate"/>
                    </div>
                    <div class="form-group">
                        <label>Match score:</label>
                        <input type="text" class="form-control" v-model="item.matchScore"/>
                    </div>
                    <div class="form-group">
                        <label>Match rank:</label>
                        <input type="text" class="form-control" v-model="item.matchRank"/>
                    </div>
                    <div class="form-group">
                        <input type="submit" class="btn btn-update" value="Add Match"/>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        components: {
            name: 'AddMatch'
        },
        data() {
            return {
                item: {
                }
            }
        },
        methods: {
            addMatch() {
                let uri = 'http://siskofasa.nl:8000/api/archerymatch/';
               this.axios.post(uri, this.item).then((response) => {
                    console.log(response.data)
                });
               console.log(this.item);
            }
        }
    }
</script>