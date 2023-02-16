<template>
  <div class="app">

    <form class="newPost" @submit.prevent>
      <h2>Добавить новый пост</h2>
      <input type="text" placeholder="Введите заголовок" v-model="title">
      <textarea cols="20" rows="5" placeholder="Введите описание" :value="body" @input="body = $event.target.value"></textarea>
      <div class="btn-control">
        <button class="btn btn-success" @click="createOrEditPost" v-show="!edit">Добавить новый пост</button>
        <button class="btn btn-warning" @click="createOrEditPost" v-show="edit">Редактировать пост</button>
        <button class="btn btn-danger" @click="cancelEdit" v-show="edit">Отмена</button>
      </div>
    </form>

    <div class="post" v-for="post in posts" :key="post.id">
      <small>ID: {{ post.id }}</small>
      <h3>{{ post.title }}</h3>
      <p>{{ post.body }}</p>
      <div class="btn-control">
        <button class="btn btn-warning" v-on:click="editPost(post)">Редактировать</button>
        <button class="btn btn-danger" @click="removePost(post)">Удалить</button>
      </div>
    </div>
    <h2 class="postEmptyHeading" v-if="posts.length === 0">Список пуст</h2>

  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [
        {id: 1001, title: "Это пост номер 1", body: "Это крутое описание поста под номером 11"},
        {id: 1002, title: "Это пост номер 2", body: "Это крутое описание поста под номером 22"},
        {id: 1003, title: "Это пост номер 3", body: "Это крутое описание поста под номером 33"},

      ],
      id: "",
      title: "",
      body: "",
      edit: false,
    }
  },
  methods: {
    cancelEdit(){
      this.id = ""
      this.title = ""
      this.body = ""
      this.edit = false
    },

    removePost(post){
      this.posts = this.posts.filter(p => p.id !== post.id)
    },

    editPost(post) {
      this.id = post.id
      this.title = post.title
      this.body = post.body
      this.edit = true
    },

    createOrEditPost(){
      let newPost = {
        id: this.id === "" ? Date.now() : this.id,
        title: this.title,
        body: this.body,
      };

      if (this.title.trim() === "")  {
        return alert('Название не должно быть пустым')
      } else if (this.body.trim() === "") {
        return alert('Описание не должно быть пустым')
      }

      if (this.edit) {

        for (let post of this.posts) {
          if (post.id === newPost.id) {
            post.title = newPost.title
            post.body = newPost.body
          }
        }
        this.edit = false

      } else {
        this.posts.push(newPost)
      }

      this.title = "";
      this.body = "";
      this.id = ""
    }

  },
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.newPost {
  padding: 10px;
  margin: 20px;
  display: flex;
  flex-direction: column;
  width: 40%;
}
.newPost h2 {
  margin-bottom: 10px;
}

.newPost input, .newPost textarea {
  border: 1px solid teal;
  margin-bottom: 10px;
  padding: 8px 5px;
}

.post {
  border: 2px solid teal;
  padding: 10px;
  margin: 20px;
  display: flex;
  flex-direction: column;
}

.post h3 {
  color: teal;
}

.post p {
  margin-top: 10px;
}

.btn-control {
  display: flex;
  align-self: end;
}

.btn {
  background: none;
  padding: 10px 10px;
  margin-left: 10px;
  cursor: pointer;
}

.btn-danger {
  border: 2px solid darkred;
  color: darkred;
}

.btn-success {
  border: 2px solid teal;
  color: teal;
}

.btn-warning {
  border: 2px solid #f39d45;
  color: #f39d45;
}

</style>