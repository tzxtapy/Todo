<template>
  <div id="todolist" class="dowebok">
          <h1>
            待办事项
            <span>一次只做一件事</span>
          </h1>

          <ul>
            <li v-for="(item,index) in mission" :key="item">
              <span :class="item.done?'Active':'' ">{{item.label}}</span>
              <div class="action">
                <van-button plain size="small" type="info" @click="btnClick(index)">DONE</van-button>
                <van-button plain size="small" type="info" v-on:click="deleteItemFromList(item)" >
                  <i aria-hidden="true">删除</i>
                </van-button>   
              </div>

            </li>
            <p class="emptylist" v-show="mission.length === 0">没有代办事项!</p>
          </ul>

          <div class="getchange">
            <span>完成的移动到底部</span>
            <van-switch v-model="checked" size="16px" v-on:click="changelist" />
          </div>

          <form v-on:submit.prevent="addItem">
            <label>添加待办事项</label>

            <input type="text" v-model="newitem">
            <button type="submit">添加</button>
          </form>
  </div>
</template>

<script>
export default {
    data(){
        return{
            newitem: '',
            checked: true,
            change: false, 
            mission:
                [
                  { label: "学习 VueJs",  done: true },
                  { label: "跑步 10 公里", done: false },
                ],
        }
                  
    },
    methods:{

                addItem: function () {
                  this.mission.push({label: this.newitem,done: false});
                  this.newitem = '';
                },
                deleteItemFromList: function (item) {
                  let index = this.mission.indexOf(item)
                  this.mission.splice(index, 1);
                },
                btnClick:function(index){
                  this.mission[index].done = !this.mission[index].done;
                },
                changelist:function (active) {
                  this.change = active;
                }
    },
    computed: {
        tochange: function () {

            if (!this.change) {
                return this.mission;
            }

            var changeArray = []
            var doneArray = this.mission.filter(function (item) { return item.done; });
            var notDoneArray = this.mission.filter(function (item) { return !item.done; });

            changeArray = [...notDoneArray, ...doneArray];
            return changeArray;
        }
    }


}
</script>

<style>
.dowebok{
  margin: 4rem auto;
  padding: 2rem 3rem 3rem;
  max-width: 500px;
  background: #FF6666;
  color: #FFF;
  box-shadow: -20px -20px 0px 0px rgb(100 100 100 / 10%);
}
#todolist h1 {
  font-weight: normal;
  font-size: 2.6rem;
  letter-spacing: 0.05em;
  border-bottom: 1px solid rgba(255, 255, 255, .3);
}
#todolist h1 span {
  display: block;
  font-size: 0.8rem;
  margin-bottom: 0.7rem;
  margin-left: 3px;
  margin-top: 0.2rem;
}
#todolist li {
  display: flex;
  margin: 0 -3rem 4px;
  padding: 1.1rem 3rem;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
}
#todolist .emptylist {
    margin-top: 2.6rem;
    text-align: center;
    letter-spacing: .05em;
    font-style: italic;
    opacity: 0.8;
}
.Active{
    text-decoration:line-through;
    opacity: .6;
    position: relative;
    transition: opacity .2s linear;
}
.getchange{
  float: right;
}
form {
  margin-top: 3rem;
  display: flex;
  flex-wrap: wrap;
}
form label {
  min-width: 100%;
  margin-bottom: .5rem;
  font-size: 1.3rem;
  
}
form input, form button {
  font-family: 'Quicksand', sans-serif;
  height: 3rem;
}
form input {
   flex-grow: 1;
   border: none;
   background: #f7f1f1;
   padding: 0 1.5em;
   font-size: initial;
   color: black;
 }
form button {
  padding: 0 1.3rem;
  border: none;
  background: #FF6666;
  color: white;
  text-transform: uppercase;
  font-weight: bold;
  border: 1px solid rgba(255, 255, 255, .3);
  margin-left: 5px;
  cursor: pointer;
  transition: background .2s ease-out;
}

</style>