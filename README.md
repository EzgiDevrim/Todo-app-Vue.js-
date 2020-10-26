# To-Do List Web App with Vue.js

### Some GraphQL code examples for using in Hasura 

```sh
mutation todoOperations {
  insert_todos(objects: {description: "liste oluştur", id: 10, isDone: false}) {
    returning {
      description
      isDone
    }
  }
  update_todos(where: {id: {_eq: 10}}, _set: {description: "liste güncelle", isDone: false}) {
    returning {
      description
      isDone
    }
  }
  delete_todos(where: {id: {_eq: 10}}) {
    returning {
      description
    }
  }
}


```

This code will return :

```sh
{
  "data": {
    "insert_todos": {
      "returning": [
        {
          "description": "liste oluştur",
          "isDone": false
        }
      ]
    },
    "update_todos": {
      "returning": [
        {
          "description": "liste güncelle",
          "isDone": false
        }
      ]
    },
    "delete_todos": {
      "returning": [
        {
          "description": "liste güncelle"
        }
      ]
    }
  }
}
```


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
