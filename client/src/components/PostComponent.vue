<template>
  <div class="container">
    <h1 class="title">Latest Posts</h1>
    <div class="create-post">
      <label for="create-post">Say Something</label>
      <input type="text" id="create-post" v-model="text" placeholder="Create a post">
      <button @click="createPost">Post!</button>
    </div>
    <hr>
    <p class="error" v-if="error">{{error}}</p>
    <div class="post-container">
      <div class="post" 
      v-for="(post,index) in posts"
      :key="post._id"
      @dblclick="deletePost(post._id)"
      >
        {{`${post.createdAt.getDate()} / ${post.createdAt.getMonth()} / ${post.createdAt.getFullYear()}`}}
        <p class='text'>{{post.text}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from '../PostService';

export default {
  name: 'PostComponent',
  data(){
    return {
      posts: [],
      error:'',
      text: ''
    }
  },
  async created() {
    try{
      this.posts = await PostService.getPosts();
    }catch(err){
      this.error = err.message;
    }
  },
  methods: {
    async createPost(){
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
    },
    async deletePost(id){
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  div.container {
    max-width: 800px;
    margin: 0 auto;
  }

  .title {
    color: #16a085;
  }

  p.error {
    border: 1px solid #ff5b5f;
    background-color: #ffc5c1;
  }

  .create-post {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 2rem;
  }

  hr {
    border: 1px solid #16a085;
  }

  .create-post label {
    color: #e67e22;
    font-size: 1rem;
    font-weight: bold;
    letter-spacing: 1px;
  }

  .create-post input {
    width: 500px;
    height: 2.1rem;
    border-radius: 10px;
    border: 1px solid #e67e22;
    padding: 0 0.8rem;
    font-size: 1rem;
    box-sizing: border-box;
    margin: 0 1rem;
    outline: none;
  }

  .create-post button {
    width: 100px;
    height: 2.1rem;
    border-radius: 10px;
    background-color: #e67e22;
    color: #fff;
    cursor: pointer;
    border: 1px solid #e67e22;
    padding: 0 0.8rem;
    font-size: 1rem;
    box-sizing: border-box;
  }

  .create-post button:hover {
    background-color: #d35400;
  }

  div.post-container {
    margin-top: 2rem;
  }

  div.post {
    position:relative;
    border: 1px solid #e67e22;
    background-color: #fff;
    padding: 10px 10px 30px 10px;
    margin-bottom: 15px;
    border-radius:10px;
    box-shadow: 0 0 1rem rgba(0,0,0,0.2);
    cursor: pointer;
    user-select:none;
  }

  div.post:hover {
    border-color: #16a085;
  }

  div.cretated-at {
    position: absolute;
    top: 0;
    left: 0;
    padding: 5px 15px 5px 15px;
    background-color: darkgreen;
    color: white;
    font-size: 13px;
  }

  p.text {
    font-size: 22px;
    font-weight: 700;
    margin-bottom: 0;
  }
</style>
