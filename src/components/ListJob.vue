<template>
  <div id="app">
    <b-card
      border-variant="info"
      header="Posts"
      align="center"
      class="container-fluid md-elevation-12"
    >
      <md-table md-card class="container-fluid md-elevation-12">
        <md-table-row align="left">
          <md-table-head>Company</md-table-head>
          <md-table-head>Position</md-table-head>
          <md-table-head>Description</md-table-head>
          <md-table-head>Salary</md-table-head>
        </md-table-row>

        <md-table-row
          v-for="post in orderedPosts"
          v-bind:key="post.name"
          align="left"
        >
          <md-table-cell>{{ post.company }}</md-table-cell>
          <md-table-cell>{{ post.title }}</md-table-cell>
          <md-table-cell>{{ post.description }}</md-table-cell>
          <md-table-cell>{{ post.salary }}</md-table-cell>
          <md-table-cell>
            <b-button
              variant="outline-info"
              @click="sendInfo(post)"
              v-b-modal.modal-xl="'job'"
              >Information</b-button
            >
            &nbsp;
            <b-button
              variant="outline-primary"
              @click="sendInfo(post)"
              v-b-modal.modal-xl="'edit'"
              >Edit</b-button
            >
            &nbsp;
            <b-button variant="outline-danger" @click="deletePost(post)">
              Remove
            </b-button>
          </md-table-cell>
        </md-table-row>
      </md-table>
    </b-card>
    <b-modal
      size="xl"
      id="job"
      :title="selectedPost.company + ' - ' + selectedPost.title"
    >
      <p></p>
    </b-modal>
    <b-modal
      size="xl"
      id="edit"
      :title="selectedPost.company + ' - ' + selectedPost.title"
    >
      <label for="input-live">Job ID</label>
      <b-form-input
        id="input-live"
        size="20"
        v-model="newJobid"
        aria-describedby="input-live-help input-live-feedback"
        trim
        :value="selectedPost.id"
      ></b-form-input>

      <label for="input-live">Title</label>
      <b-form-input
        id="input-live"
        size="20"
        v-model="newTitle"
        aria-describedby="input-live-help input-live-feedback"
        :value="selectedPost.title"
      ></b-form-input>

      <label for="input-live">Company</label>
      <b-form-input
        id="input-live"
        size="20"
        v-model="newCompany"
        aria-describedby="input-live-help input-live-feedback"
        :value="selectedPost.company"
      ></b-form-input>

      <label for="input-live">Description</label>
      <b-form-textarea
        id="textarea-default"
        size="20"
        v-model="newDescription"
        aria-describedby="input-live-help input-live-feedback"
        :value="selectedPost.description"
      ></b-form-textarea>

      <label for="input-live">Salary</label>
      <b-form-input
        id="input-live"
        size="20"
        type="number"
        v-model="newSalary"
        aria-describedby="input-live-help input-live-feedback"
        trim
        :value="selectedPost.salary"
      ></b-form-input>

      <br />
      <b-button variant="outline-success">
        Edit Post
      </b-button>
    </b-modal>
  </div>
</template>

<script>
import { db } from "../firebase";
import _ from "lodash";

export default {
  name: "TableCard",
  computed: {
    orderedPosts: function() {
      return _.orderBy(this.posts, ["created"], ["desc"]);
    },
  },
  data() {
    return {
      posts: [],
      selectedPost: "",
      newJobid: "",
      newTitle: "",
      newCompany: "",
      newDescription: "",
      newSalary: "",
    };
  },
  firestore() {
    return {
      posts: db.collection("jobs"),
    };
  },
  methods: {
    deletePost: function(post) {
      this.$firestore.posts.doc(post[".key"]).delete();
    },
    // editPost: function(post) {

    // },
    //working
    sendInfo(post) { 
      console.log(post)
      this.newJobid = post.id
      this.newTitle = post.title;
      this.newCompany = post.company;
      this.newDescription = post.description;
      this.newSalary = post.salary;
      this.selectedPost = post;
      
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
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
