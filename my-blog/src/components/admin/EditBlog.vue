<template>
<div id="add-blog"  class="panel panel-default">
    <div class="panel-heading">编辑博客</div>
    <form v-if="!submmited">
        <label for="">博客标题</label>
        <input type="text" class="form-control" v-model="blog.title" required>
        <label for="">博客分类</label>
        <select v-model="categoryTip" class="form-control">
            <option v-for="(categoryb,index) in categoryBox" :key="index" :value="categoryb.id">
                {{categoryb.title}}
            </option>
        </select>
        <label for="">博客关键字</label>
        <input type="text" class="form-control" v-model="keywordsBox" required>
        <label for="">博客描述</label>
        <textarea class="form-control" rows="3" v-model="blog.w_description"></textarea>
        <label for="">博客内容</label>
        <textarea class="form-control" rows="10" v-model="blog.content"></textarea>

        <label for="">作者：</label>
        <input type="text"  class="form-control" v-model="blog.author" required>

        <button v-on:click.prevent="post" class="btn btn-primary">编辑博客</button>
    </form>
    <div class="panel-body" v-if="submmited">
        <h3>您的博客发布成功！</h3>
        <div id="preview">
            <h3>博客总览</h3>

            <h4>博客标题：</h4>
            <p>{{blog.title}}</p>
            <h4>博客分类：</h4>
            <p>{{blog.categories.title}}</p>
            <h4>博客关键字：</h4>
            <p>{{blog.keywords}}</p>
            <h4>博客描述：</h4>
            <p>{{blog.w_description}}</p>
            <h4>博客内容：</h4>
            <p>{{blog.content}}</p>
            <h4>作者：</h4>
            <p>{{blog.author}}</p>
        </div>
    </div>
</div>
  
</template>

<script>
export default {
    // https://jsonplaceholder.typicode.com/
    // https://jsonplaceholder.typicode.com/posts
    // https://dlovel-fd248.firebaseio.com/posts.json
  name: 'edit-blog',
  data () {
    return {
      id:this.$route.params.id,
      newObj:{},
      blog:{},
      submmited:false,
      categoryBox:[],
      categoryTip:"",
      keywordsBox:"",
      api:this.Glogal.api, //借口地址
    }
  },
  methods:{
      fetchData(){
        // this.$http.get("https://dlovel-fd248.firebaseio.com/posts/" + this.id + ".json")
        this.$http.get(this.api+"db")
        .then(function(res){
            var blogsArray = [];
            for(let key in res.body.posts){
                if(res.body.posts[key].id == this.id){
                    this.blog = res.body.posts[key]
                    this.categoryTip = res.body.posts[key].categories.id
                    this.keywordsBox = this.blog.keywords.toString()
                    
                }
            }
            var categoriesArray = [];
            for(let key in res.body.categories){
                categoriesArray.push(res.body.categories[key])
            }
            this.categoryBox = categoriesArray
            
        })
      },
      post:function(){
          this.blog.categories.id = this.categoryTip
          this.keywordsBox=this.keywordsBox.replace(/，/ig,','); 
          this.blog.keywords = this.keywordsBox.split(",")

          this.$http.put(this.api + "posts/" + this.id,this.blog)
          .then(function(data){
            this.submmited = true
            this.$router.push({path:'../BlogList'})
            window.location.reload()
          })
      }
  },
  created(){
      this.fetchData()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#add-blog *{
    box-sizing:border-box;
}

#add-blog form{
    padding:20px;
}
label{
    display:block;
    margin:20px 0 10px;
}
/* input[type=text],textarea,select{
    display:block;
    width:100%;
    padding:8px;
} */
textarea{
    max-width:100%;
}
#checkboxes label{
    display:inline-block;
    margin-top:0;
}
#checkboxes input{
    display:inline-block;
    margin-right:10px;
}
button{
    display:block;
    margin:20px 0;
    /* background:crimson;
    color:#FFF;
    border:0;
    padding:14px;
    border-radius:4px; 
    font-size:18px;*/
    cursor:pointer;
}

#preview{
    padding:10px 20px;
    margin:30px 0;
}
h3{
    margin-top:10px;
}
article{
    max-width:100%;
}
.row{
        margin-top:52px;
        position:relative;
    }
    .leftsilder{
        background-color: #f5f5f5;
        border-right: 1px solid #eee;
        padding:20px 0;
    }
    .active a{
        color: #fff;
        background-color: #428bca;
    }
    .active a:hover{
        color: #fff;
        background-color: #428bca;
    }
</style>
