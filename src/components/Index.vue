// Index.vue

<template>
    <div id="overview">
        <div class="title">
            <h1>Archery Matches</h1>
        </div>

        <div class="tiles">
            <div v-for="item in items" :key="item._id">
                <p> {{item.matchName}}</p>
                <b-collapse :id="'collapse-'+item._id" class="mt-2">

                    <form v-on:submit.prevent="updateItem(item)">
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
                            <input type="submit" class="btn btn-primary btn-update" value="Update Item"/>
                        </div>
                    </form>

                    <!--                <button><router-link :to="{name: 'Edit', params: { id: item._id }}" class="btn btn-primary">Edit</router-link></button>-->
                    <button class="btn btn-danger"  v-on:click="deleteItem(item)">Delete</button>
                </b-collapse>
                <b-button v-b-toggle="'collapse-' + item._id" variant="primary" size="sm">Details</b-button>
            </div>
        </div>

    </div>
</template>

<script>

    export default {
        data(){
            return{
                items: []
            }
        },

        created: function()
        {
            this.fetchItems();
        },

        methods: {
            fetchItems()
            {
                let uri = 'http://siskofasa.nl:8000/api/archerymatch';
                this.axios.get(uri).then((response) => {
                    this.items = response.data.items;
                });
            },

            deleteItem(item)
            {
                console.log("hallo?");
                let uri = 'http://siskofasa.nl:8000/api/archerymatch/'+item._id;
                this.items.splice(this.items.indexOf(item), 1);

                this.axios.delete(uri);
            },

            updateItem(item)
            {
                let uri = 'http://siskofasa.nl:8000/api/archerymatch/' +item._id;
                this.axios.put(uri, item, { headers: {
                        'Content-Type': 'application/json'
                    } }).then((response) => {
                    console.log(response.data)
                });
            }
        }
    }
</script>


