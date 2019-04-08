<template>
	<div>
	<input type="text" class="todo-input" placeholder="Введи что нужно сделать, а после нажми кнопку 'Добавить'  " v-model="firstTodo">
  <button class="add-item-btn" v-on:click="addNewTodo(todo)">Добавить</button>

	<div v-for="(todo, i) in todosFiltered" :key="todo.id" class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="todo.completed">
      <div v-if="!todo.editing" v-on:dblclick="editTodo(todo)" class="todo-item-label" v-bind:class="{ completed : todo.completed }"> {{ todo.title }}</div>
      <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
    </div>
    <div class="remove-item " >
       <button class="remove-item-btn" v-on:click="removeTodo(i)">Удалить</button>
    </div>
	</div>

  <div class="extra-container">
       <div><label><input type="checkbox" v-bind="!anyRemaining" @change="checkAllTodos">Зачеркнуть все</label></div>
    <div>{{ remaining }} в Todo листе</div>
  </div>

  <div class="extra-container">
    <div>
      <button v-bind:class="{ active: filter == 'all' }" v-on:click="filter = 'all'">Все</button>
      <button v-bind:class="{ active: filter == 'active' }" v-on:click="filter = 'active'">Не Выполнено</button>
      <button v-bind:class="{ active: filter == 'completed' }" v-on:click="filter = 'completed'">Выполнено</button>
    </div>
  </div>

	</div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
  	return {
    firstTodo: '',
    idForTodo: 3,
    beforeEditCache: '',
    filter: 'all',
	  todos: [
		  {
			  'id': 1,
        'title': 'Сделать часть первую тестового задания',
        'completed': false,
        'editing': false,
		  },
      {
        'id': 2,
        'title': 'Попытаться сделать Todo лист на Vue, с помощью гайдов, документации и тд',
        'completed': false,
        'editing': false,
      },
    ]
      }
    },
    computed: {
      remaining() {
        return this.todos.filter(todo => !todo.completed).length
    },
      anyRemaining() {
        return this.remaining != 0
    },
     todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
      }
    },
    directives: {
      focus: {
        inserted: function(el) {
          el.focus()
        }
      }
    },
	  methods: {
	  	addNewTodo(todo) {
        if(this.firstTodo.trim().length == 0) {
          return 
        }
			  this.todos.push({
          id: this.idForTodo,
          title: this.firstTodo,
          completed: false,
      })

      this.firstTodo = ''
      this.idForTodo++
      },
      editTodo(todo) {
        this.beforeEditCache = todo.title
        todo.editing = true;
      },
      doneEdit(todo) {
        if(todo.title.trim() == '') {
          todo.title = this.beforeEditCache
        }
       todo.editing = false;
      },
      cancelEdit(todo) {
        todo.title = this.beforeEditCache
        todo.editing = false;
      },
      removeTodo(i) {
        this.todos.splice(i, 1) /* delete 1 item from array*/
      },
      checkAllTodos () {
        this.todos.forEach((todo) => todo.completed = event.target.checked)
      }
	}
}
</script>


<style lang="less">

.todo-input {
	width: 100%;
	border: 1px solid rgba(102, 102, 102, 0.192);
	font-size: 18px;
	padding: 10px 18px;
	margin-bottom: 20px;
	transition: 0.2s ease-in-out !important;

	&:focus {
		outline: none;
	}

	&:focus::-webkit-input-placeholder {
		opacity: 0;
		transition: 0.2s ease-in-out !important;
		will-change: transform, opacity;
	}

	&::-webkit-input-placeholder {
  		transition: 0.2s ease-in-out !important;
  		will-change: transform, opacity;
	}

	&::-moz-placeholder {
		transition: all 0.2s ease-in-out !important;
		will-change: transform, opacity;
	}
		/* Firefox 19+ */
	&:-moz-placeholder {
		transition: all 0.2s ease-in-out !important;
		will-change: transform, opacity;
	}
		/* Firefox 18- */
	&:-ms-input-placeholder {
		transition: all 0.2s ease-in-out !important;
		will-change: transform, opacity;
	}

}

  .todo-item {
    width: 100%;
    margin-bottom: 12px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .remove-item-btn {
    background: #34475D;
    color: #fff;
    border: none;
    padding: 5px 5px;
    border-radius: 5px;
    transition: .1s ease-in-out;
    cursor: pointer;

    &:focus {
      outline: none;
    }

    &:hover {
      background: #41B883;
      transition: .1s ease-in-out;
    }
  }
    .todo-item-left {
      display: flex;
      align-items: center;
    }

    .todo-item-label {
      padding: 0px;
      border: 1px solid white;
      margin-left: 12px;
    }

    .todo-item-edit {
      width: 100%;
      border: 1px solid rgba(102, 102, 102, 0.192);
      font-size: 18px;
      padding: 10px 18px;
      margin-bottom: 20px;
      transition: 0.2s ease-in-out !important;

    &:focus {
      outline: none;
  	}

    &:focus::-webkit-input-placeholder {
      opacity: 0;
      transition: 0.2s ease-in-out !important;
      will-change: transform, opacity;
	  }

	  &::-webkit-input-placeholder {
  		transition: 0.2s ease-in-out !important;
  		will-change: transform, opacity;
	  }

  	&::-moz-placeholder {
		  transition: all 0.2s ease-in-out !important;
		  will-change: transform, opacity;
    }
    
		/* Firefox 19+ */
	  &:-moz-placeholder {
		  transition: all 0.2s ease-in-out !important;
		  will-change: transform, opacity;
    }
    
		/* Firefox 18- */
	  &:-ms-input-placeholder {
		  transition: all 0.2s ease-in-out !important;
		  will-change: transform, opacity;
    }  
  }

  .completed {
    text-decoration: line-through;
    color: black;
  }

  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
  }
  
  .add-item-btn {
    display: flex;
    float: right;
    margin-bottom: 15px;
    background: #41B883; 
    color: #fff;
    border: none;
    padding: 10px 15px;
    cursor: pointer;
    transition: all 0.2s ease-in-out !important;

    &:hover {
      background: #34475D;
      transition: .1s ease-in-out;
      transition: all 0.2s ease-in-out !important;
    }
    
    &:focus {
      outline: none;
    }
  }

  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    border: none;
    
    &:hover {
      background: lightgreen;
    }
    &:focus {
      outline: none;
    }
  }
  .active {
    background: lightgreen;
  }


</style>
