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
                        <label>Match name:</label>
                        <input type="text" class="form-control" v-model="item.matchName"/>
                    </div>
                    <div class="form-group">
                        <label>Match date:</label>
                        <input type="text" class="form-control" v-model="item.matchDate" />
                    </div>
                    <div class="form-group">
                        <label>Match score:</label>
                        <input type="text" class="form-control" v-model="item.matchScore" />
                    </div>
                    <div class="form-group">
                        <label>Match rank:</label>
                        <input type="text" class="form-control" v-model="item.matchRank" />
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
                this.axios.post(uri, this.item).then((response) => {
                    this.$router.push({name: 'Index'});
                });
            }
        }
    }
</script>