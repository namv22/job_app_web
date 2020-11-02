<template>
<div id="app">
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
} from '../firebase';
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
