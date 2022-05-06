<template>
  <div>
    <div class="container">
      <h1 class="title">Todo List</h1>
      <div class="input-group mb-3">
        <input
          type="text"
          class="form-control"
          placeholder="名前を入力してください"
          aria-label="名前を入力してください"
          aria-describedby="button-addon2"
          v-model="inputtext"
        />
        <button
          class="btn btn-outline-secondary"
          type="button"
          id="button-addon2"
          @click="add"
        >
          追加
        </button>
      </div>
      <div>
        <h4>全{{total}}件中、{{finish}}件完成した</h4>
      </div>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">ID</th>
            <th scope="col">Name</th>
            <th scope="col">Status</th>
            <th scope="col">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.id">
            <th scope="row">{{item.id}}</th>
            <td>{{item.name}}</td>
            <td>
              <button
                type="button"
                class="btn btn-primary"
                @click="$store.dispatch('changeState' , {item:item})"
              >
                <span v-if="item.status == 0"> todo </span>
                <span v-else class="done"> done </span>
              </button>
            </td>
            <td>
              <button
                type="button"
                class="btn btn-danger"
                @click="$store.dispatch('remove' , {item:item})"
              >
                削除
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import {mapState} from 'vuex'

export default {
  data(){
    return{
      inputtext:'',
    }
  },

computed : {
  ...mapState(["items"]),
  finish(){
    let sum = 0;
    for (const item of this.items){
      sum += Number(item.status);
    }
    return sum;
  },
  total(){
    // return this.$store.state.items.length;
    return this.items.length;
  }
  },

created(){
  this.$store.dispatch('getItems');
},

methods : {
  add(){
    if (this.inputtext !== '' && this.inputtext.length<=64){
    this.$store.dispatch('add',{name:this.inputtext , status: 0});
    this.inputtext = '' ;
    }else {
      alert("入力した文字の長さが正しくない！");
      this.inputtext = '' ;
    }
  }
}
}
</script>

<style scoped>
div {
  background: rgb(250, 234, 236);
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.title {
  font-family: "Raleway", sans-serif;
  font-size: 80px;
  color: rgb(190, 10, 115);
}

.done {
  color: rgb(26, 6, 73);
}
</style>
/* eslint-enable */
