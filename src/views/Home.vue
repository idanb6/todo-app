<template>
  <div class="home">
       <h1>To do List App</h1>
       <!-- <h1>{{data[0]}}</h1> -->
       <p>{{total}} משימות</p>
       <p>{{sumDone}} משימות שבוצעו</p>
              <p>{{sumNotDone}} משימות שנותרו</p>
       <div>
         <form action="">
<input type="text" v-model="name">
<input type="hidden" v-model="hiddenid">

<button type="submit" v-if="but" @click="addTask()">Send</button>
<button type="button" v-else @click="sendEdit()">edit</button>
         </form>


       </div>
<div id="todo-list">
  <span class="todo-wrap" v-for="d in data" :key="d.id">
<input type="checkbox" checked v-if="d.done">
    <input type="checkbox" v-else>
    <label  class="todo" @click="isDone(d)">
      <i class="fa fa-check"></i>
     {{d.task_name}}
    </label>
    <span class="delete-item" title="remove">
      <i class="fa fa-times-circle" @click="dell(d.id)"></i>
    </span>
     <span class="edit-item" title="edit">
        <i class="fas fa-edit" @click="edit(d)"></i>
    </span>
  </span>
</div>
  </div>
</template>

<script>


export default {
  name: 'home',
  components: {

  },
  data() {
      return{
          data:[],  
          name,
          hiddenid:'',
          i:0,
          sumDone:0,
          sumNotDone:0,
          total:0,
          but:true,
          // newTasks:
          //     {
          //   task_name:'',
          //   done:false
          //     }

      }
  },
  created() {
      this.fetchData();
  },
methods: {
      dell(r){
        if(confirm('Are you sure?')){
 this.$http.delete("http://127.0.0.1:8000/api/tasks/"+r)
                .then(()=>{
                    location.reload();
                               })
        }
      },
      edit(r){
          this.name = r.task_name;
          this.hiddenid = r.id;
          this.but=false;
          
      },
      sendEdit(){
        // alert(this.hiddenid);
        //  alert(this.name);
        var temp_id=this.hiddenid;
         var temp_name=this.name;
          this.$http.put("http://127.0.0.1:8000/api/tasks/"+temp_id+"?task_name="+temp_name)
                .then(()=>{
                       location.reload();
                               })
      },
    isDone(r){
        var don = r.done;
        if(don == true){
            don = '0';
        }else if(don == false){
            don = '1';
        }
                 this.$http.put("http://127.0.0.1:8000/api/tasks/"+r.id+"?done="+don)
                .then(()=>{
                       location.reload();
                               })
      },
 fetchData(){ 
  this.$http.get("http://127.0.0.1:8000/api/tasks").then((res)=>{
   this.data=res.data;
    this.total=this.total+this.data.length; 
    for(this.i=0; this.i< this.total; this.i++) {
                if(this.data[this.i].done)
                {
                     this.sumDone=this.sumDone+1;
                }else{
                   this.sumNotDone=this.sumNotDone+1;
                }      
               }
    })
  },
   toFromData(obj){
              var fd= new FormData();
              for(var i in obj){
                fd.append(i,obj[i]);
              }
              return fd;
            },
 addTask() {
var data = new FormData();
data.append("done", "0");
data.append("task_name", this.name);
                 this.$http.post("http://127.0.0.1:8000/api/tasks",data)
                .then((res)=>{  
                                    console(res);
                              })
            },
}
}
</script>
<style>
#home {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
*{
/*transition*/
-webkit-transition:.25s ease-in-out;
   -moz-transition:.25s ease-in-out;
     -o-transition:.25s ease-in-out;
        transition:.25s ease-in-out;
font-family:helvetica neue,helvetica,arial,sans-serif;
font-size:18px;
line-height:18px;
box-sizing:border-box;
margin:0;
}
body{
background:#f8f8f8;
}
h1{
text-align:center;
padding:50px 0;
font-size:30px;
margin:0;
font-weight:200;
color:#454545;
}
h1 .fa-check{
font-size:30px;
margin-right:10px;
color:#0eb0b7;
}
#todo-list{
width:500px;
margin:0 auto 50px auto;
padding:50px;
background:white;
position:relative;
/*box-shadow*/
-webkit-box-shadow:0 1px 4px rgba(0, 0, 0, 0.3);
   -moz-box-shadow:0 1px 4px rgba(0, 0, 0, 0.3);
        box-shadow:0 1px 4px rgba(0, 0, 0, 0.3);
/*border-radius*/
-webkit-border-radius:5px;
   -moz-border-radius:5px;
        border-radius:5px;
}
#todo-list:before{
content:"";
position:absolute;
z-index:-1;
/*box-shadow*/
-webkit-box-shadow:0 0 20px rgba(0,0,0,0.4);
   -moz-box-shadow:0 0 20px rgba(0,0,0,0.4);
        box-shadow:0 0 20px rgba(0,0,0,0.4);
top:50%;
bottom:0;
left:10px;
right:10px;
/*border-radius*/
-webkit-border-radius:100px / 10px;
   -moz-border-radius:100px / 10px;
        border-radius:100px / 10px;
}
.todo-wrap{
display:block;
position:relative;
padding-left:35px;
/*box-shadow*/
-webkit-box-shadow:0 2px 0 -1px #ebebeb;
   -moz-box-shadow:0 2px 0 -1px #ebebeb;
        box-shadow:0 2px 0 -1px #ebebeb;
}
.todo-wrap:last-of-type{
/*box-shadow*/
-webkit-box-shadow:none;
   -moz-box-shadow:none;
        box-shadow:none;
}
input[type="checkbox"]{
position:absolute;
height:0;
width:0;
opacity:0;
top:-600px;
}
.todo{
display:inline-block;
font-weight:200;
padding:10px 5px;
height:37px;
position:relative;
}
.todo:before{
content:'';
display:block;
position:absolute;
top:calc(50% + 2px);
left:0;
width:0%;
height:1px;
background:#cd4400;
/*transition*/
-webkit-transition:.25s ease-in-out;
   -moz-transition:.25s ease-in-out;
     -o-transition:.25s ease-in-out;
        transition:.25s ease-in-out;
}
.todo:after{
content:'';
display:block;
position:absolute;
z-index:0;
height:18px;
width:18px;
top:9px;
left:-25px;
/*box-shadow*/
-webkit-box-shadow:inset 0 0 0 2px #d8d8d8;
   -moz-box-shadow:inset 0 0 0 2px #d8d8d8;
        box-shadow:inset 0 0 0 2px #d8d8d8;
/*transition*/
-webkit-transition:.25s ease-in-out;
   -moz-transition:.25s ease-in-out;
     -o-transition:.25s ease-in-out;
        transition:.25s ease-in-out;
/*border-radius*/
-webkit-border-radius:4px;
   -moz-border-radius:4px;
        border-radius:4px;
}
.todo:hover:after{
/*box-shadow*/
-webkit-box-shadow:inset 0 0 0 2px #949494;
   -moz-box-shadow:inset 0 0 0 2px #949494;
        box-shadow:inset 0 0 0 2px #949494;
}
.todo .fa-check{
position:absolute;
z-index:1;
left:-31px;
top:0;
font-size:1px;
line-height:36px;
width:36px;
height:36px;
text-align:center;
color:transparent;
text-shadow:1px 1px 0 white, -1px -1px 0 white;
}
:checked + .todo{
color:#717171;
}
:checked + .todo:before{
width:100%;
}
:checked + .todo:after{
/*box-shadow*/
-webkit-box-shadow:inset 0 0 0 2px #0eb0b7;
   -moz-box-shadow:inset 0 0 0 2px #0eb0b7;
        box-shadow:inset 0 0 0 2px #0eb0b7;
}
:checked + .todo .fa-check{
font-size:20px;
line-height:35px;
color:#0eb0b7;
}
/* Delete Items */

.delete-item{
display:block;
position:absolute;
height:36px;
width:36px;
line-height:36px;
right:20px;
top:0;
text-align:center;
color:#d8d8d8;
opacity:0;
}
.todo-wrap:hover .delete-item{
opacity:1;
}
.delete-item:hover{
color:#cd4400;
}
.edit-item{
display:block;
position:absolute;
height:36px;
width:36px;
line-height:36px;
right:0;
top:0;
text-align:center;
color:#d8d8d8;
opacity:0;
}
.todo-wrap:hover .edit-item{
opacity:1;
}
.edit-item:hover{
color:#42b983;
}

.input-todo{
border:none;
outline:none;
font-weight:200;
position:relative;
top:-1px;
margin:0;
padding:0;
width:100%;
}
.editing{
  height:0;
  overflow:hidden;
}

.editing.todo-wrap {
  box-shadow:0 0 400px rgba(0,0,0,.8),inset 0 0px 0 2px #ebebeb;
}

</style>