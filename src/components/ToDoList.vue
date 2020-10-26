<template>
  <div class="container">
    <div class="title">
      <h2>My To-Do List</h2>
    </div>
    <div class="container-colored">
      <div class="add-item">
        <div class="add-icon" @click="addItem">
          <i class="fas fa-plus"></i>
        </div>
        <form @submit.prevent="addItem">
          <input type="text" placeholder="Add a todo" v-model="entry">
        </form>
        <div class="feature-icon" :class="{'star': entryFavorite}">
          <i v-if="!entryFavorite" class="far fa-star" @click="entryFavorite = !entryFavorite"></i>
          <i v-else class="fas fa-star" @click="entryFavorite = !entryFavorite"></i>
        </div>
      </div>
      <div class="todo-list">
        <div class="item" :class="{'show': item.show}" v-for="(item, index) in todoList" :key="index">
          <div class="item-checkbox">
            <i v-if="!item.complete" class="fas fa-square" @click="completeItem(item)"></i>
            <i v-else class="fas fa-check-square"></i>
          </div>
          <div class="item-title">
            <input type="text" class="item-type" placeholder="Edit a todo" @keyup.enter="editItem($event, item)" v-bind:value="item.title">
          </div>
          <div class="feature-icon" :class="{'star': item.favorite}">
            <i v-if="!item.favorite" class="far fa-star" @click="setFavoriteItem(item)"></i>
            <i v-else class="fas fa-star" @click="item.favorite = !item.favorite"></i>
          </div>
          <div class="deleted" :class="{'del': item.deleted}">
            <i v-if="!item.deleted" class="far fa-circle" @click="deleteItem(item)"></i>
            <i v-else class="fas fa-circle"></i>
          </div>
        </div>
      </div>
      <div class="show-completed" v-if="completedToDoList.length > 0">
        <div class="button" @click="showCompletedList = !showCompletedList">
          <span v-if="!showCompletedList">show</span><span v-else>hide</span> 
          completed to-do's
        </div>
      </div>
      <div class="todo-list complete-list" v-if="showCompletedList">
        <div class="item" :class="{'show': item.show}" v-for="(item, index) in completedToDoList" :key="index">
          <div class="item-checkbox">
            <i v-if="!item.complete" class="fas fa-square"></i>
            <i v-else class="fas fa-check-square" @click="uncompleteItem(item)"></i>
          </div>
          <div class="item-title">
            {{ item.title }}
          </div>
          <div class="feature-icon" :class="{'star': item.favorite}">
            <i v-if="!item.favorite" class="far fa-star"></i>
            <i v-else class="fas fa-star"></i>
          </div>
          <div class="deleted" :class="{'del': item.deleted}">
            <i v-if="!item.deleted" class="far fa-circle" @click="deleteItem(item)"></i>
            <i v-else class="fas fa-circle"></i>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { setTimeout } from 'timers';
export default {
  data: () => {
    return {
      entry: '',
      entryFavorite: false,
      todoList: [],
      completedToDoList: [],
      showCompletedList: false,
    };
  },
  methods: {
    addItem() {
      if ( this.entry !== '' ) {
        const date = new Date();
        const newEntry = {
          id: date.getTime(),
          title: this.entry,
          favorite: this.entryFavorite,
          complete: false,
          deleted: false,
          show: false,
        };
        if ( newEntry.favorite ) {
          // push new item on first position
          this.todoList.splice(0, 0, newEntry);
        } else {
          // push new item on last position
          this.todoList.push(newEntry);
        }
        setTimeout(() => {
          this.todoList.find( (element) => element.id === newEntry.id ).show = true;
        }, 10);
      }
      this.entry = '';
      this.entryFavorite = false;
    },
    editItem(e, item) {
      item.title = e.target.value;
      item.show = false;
      setTimeout(() => {
        const index = this.todoList.findIndex( (element) => element.id === item.id );
        this.todoList.splice(index, 1);
        this.todoList.splice(0, 0, item);
        item.show = true;
      }, 500);
    },
    setFavoriteItem(item) {
      item.favorite = !item.favorite;
      item.show = false;
      setTimeout(() => {
        const index = this.todoList.findIndex( (element) => element.id === item.id );
        this.todoList.splice(index, 1);
        this.todoList.splice(0, 0, item);
        item.show = true;
      }, 500);
    },
    completeItem(item) {
      item.complete = !item.complete;
      item.show = false;
      setTimeout(() => {
        this.completedToDoList.push(item);
        const index = this.todoList.findIndex( (element) => element.id === item.id );
        this.todoList.splice(index, 1);
        item.show = true;
      }, 500);
    },
    uncompleteItem(item) {
      item.complete = !item.complete;
      item.show = false;
      setTimeout(() => {
        if ( item.favorite ) {
          this.todoList.splice(0, 0, item);
        } else {
          this.todoList.push(item);
        }
        const index = this.completedToDoList.findIndex( (element) => element.id === item.id );
        this.completedToDoList.splice(index, 1);
        item.show = true;
      }, 500);
    },
    deleteItem(item) {
      item.deleted = !item.deleted;
      item.show = false;
      setTimeout(() => {
        const completedListIndex = this.completedToDoList.findIndex( (element) => element.id === item.id );
        const listIndex = this.todoList.findIndex( (element) => element.id === item.id );
        if (completedListIndex !== -1) {
          this.completedToDoList.splice(completedListIndex, 1);
        } else {
          this.todoList.splice(listIndex, 1);
        }
        item.show = true;
      }, 500);
    },
  },
};
</script>
