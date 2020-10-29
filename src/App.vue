<template>
<div id="app">
    <b-card border-variant="success" header="Add Post" class="card-add">
        <label for="input-live">Name:</label>
        <b-form-textarea id="textarea-default" size="20" v-model="newName" :state="nameState" aria-describedby="input-live-help input-live-feedback" placeholder="Enter post name" trim></b-form-textarea>

        <!-- This will only be shown if the preceding input has an invalid state -->
        <b-form-invalid-feedback id="input-live-feedback">
            Enter name for post
        </b-form-invalid-feedback>

        <label for="input-live">Youtube ID:</label>
        <b-form-input id="input-live" size="20" v-model="newYT" :state="ytState" aria-describedby="input-live-help input-live-feedback" placeholder="Enter post Youtube ID" trim></b-form-input>

        <!-- This will only be shown if the preceding input has an invalid state -->
        <b-form-invalid-feedback id="input-live-feedback">
            Enter Youtube ID for post
        </b-form-invalid-feedback>

        <b-form-group label="Tag">
            <b-form-radio v-model="newTag" name="some-radios" value="game">Game</b-form-radio>
            <b-form-radio v-model="newTag" name="some-radios" value="music">Music</b-form-radio>
            <b-form-radio v-model="newTag" name="some-radios" value="meme">Meme</b-form-radio>
        </b-form-group>
        <div class="mt-3">Selected: <strong>{{ newTag }}</strong></div>

        <br />
        <b-button variant="outline-success" @click="addPost">
            Add Post
        </b-button>
    </b-card>
    <br /><br />
    <b-card border-variant="info" header="Posts" align="center" class="container-fluid md-elevation-12">
        <md-table md-card class="container-fluid md-elevation-12">

            <md-table-row align="left">
                <md-table-head>Company</md-table-head>
                <md-table-head>Position</md-table-head>
                <md-table-head>Description</md-table-head>
                <md-table-head>Salary</md-table-head>
            </md-table-row>

            <md-table-row v-for="post in orderedPosts" v-bind:key="post.name" align="left">
                <md-table-cell>{{ post.company }}</md-table-cell>
                <md-table-cell>{{ post.title }}</md-table-cell>
                <md-table-cell>{{ post.description }}</md-table-cell>
                <md-table-cell>{{ post.salary }}</md-table-cell>
            </md-table-row>
        </md-table>
    </b-card>
</div>
</template>

<script>
import {
    db
} from './firebase';
import _ from 'lodash';

export default {
    name: 'TableCard',
    computed: {
        nameState() {
            return this.newName.length > 0 ? true : false
        },
        ytState() {
            return this.newYT.length > 0 ? true : false
        },
        orderedPosts: function () {
            return _.orderBy(this.posts, ['date'], ['desc'])
        },
    },
    data() {
        return {
            posts: [],
            newName: '',
            newYT: '',
            newTag: '',
        }
    },
    firestore() {
        return {
            posts: db.collection('jobs'),
        }
    },
    methods: {
        addPost: function () {
            this.$firestore.posts.add({
                name: this.newName,
                yturl: this.newYT,
                tag: this.newTag,
                date: new Date()
            });
            this.newName = '';
            this.newYT = '';
            this.newTag = '';
        },
        deletePost: function (post) {
            this.$firestore.posts.doc(post['.key']).delete();
        }
    }
}
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

.card-add {
    width: 500px;
    position: relative;
    margin-left: auto;
    margin-right: auto;
}

.card-post {
    width: 800px;
    position: relative;
    margin-left: 50%;
    margin-right: 50%;
}
</style>
