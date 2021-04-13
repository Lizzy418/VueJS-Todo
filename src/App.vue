<template>
    <div id="app">
        <TodoHeader></TodoHeader>
        <TodoInput @addTodo="addTodo"></TodoInput>
        <TodoList v-bind:propsdata="todoItems" @updateTodo="updateTodo" @removeTodo="removeTodo"></TodoList>
        <TodoFooter @removeAll="clearAll"></TodoFooter>
    </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data() {
    return {
      todoItems: [],
    }
  },
  methods: {
    clearAll() {
      localStorage.clear();
      this.todoItems=[];
    },
    addTodo(todoItem){
      const max_key = this.maxKey();
      //로컬 스토리지에 데이터를 추가하는 로직
      localStorage.setItem(max_key, todoItem);
      this.todoItems.push({'t_key':max_key, 'item': todoItem});
    },
    // created() {
    // if(localStorage.length>0) {
    //     for(var i=0; i<localStorage.length; i++) {
    //         this.todoItems.push(localStorage.key(i));
    //     }
    //   }
    // },
    removeTodo(index) {
      localStorage.removeItem(this.todoItems[index]['t_key']);
      this.todoItems.splice(index, 1);
    },
    updateTodo(index, newItem) {
      localStorage.removeItem(this.todoItems[index]['t_key']);
      localStorage.setItem(this.todoItems[index]['t_key'], newItem);
      this.todoItems[index]['item'] = newItem;
    },
    maxKey(){
      let max_key = 0
      for(var i =0; i < localStorage.length; i++) {
        let key_value = parseInt(localStorage.key(i));
        if(max_key < key_value){
          max_key = key_value;
        }
      }
      // 키는 MAX 값보다 1큰 값
      max_key++;

      if(max_key < 10) {
        max_key = '000' + max_key;
      }
      else if (max_key <100) {
        max_key = '00' + max_key;
      }
      else if (max_key < 1000) {
        max_key = '0' + max_key;
      }
      else {
        max_key = '' + max_key;
      }

      return max_key;
    },
    sortByKey(array, key) {
      return array.sort(function(a, b) {
          var x = a[key]; var y = b[key];
          return ((x < y) ? -1 : ((x > y) ? 1 : 0));
      })
    },
  },
  created() {
        if(localStorage.length >0) {
            for(var i=0; i < localStorage.length; i++) {
              let key = localStorage.key(i);
              if(key !=='loglevel:webpack-dev-server') {
                this.todoItems.push({'t_key':key,'item':localStorage.getItem(key)});
              }
              
            }
        }
        this.todoItems = this.sortByKey(this.todoItems,'t_key');
  },

  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter,
  }
}
</script>

<style>
  body {
    text-align: center;
    background-color: #f3ece8;
    /* background-color: #f6f6f8;       */
  }
  input {
      border-style: groove;
      width: 200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
      box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
  }

</style>