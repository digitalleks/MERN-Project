# MERN-Project
DIO-MERN PROJECT

Update Ubuntu on the EC2 instance

Download NodeJs package with the command below:
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
Install Node.Js with the command below:
```bash
sudo apt-get install -y nodejs
```
Verify the node installation with the command below:
```bash
node -v 
```
Verify the node installation with the command below:
```bash
npm -v 
```
The version output is confirmed [here](https://user-images.githubusercontent.com/61512079/173152254-7dfdc33a-af7b-44e8-8d13-eed5bd44975e.PNG)

Create a new directory for your To-Do project:
```bash
mkdir Todo
```
Now change your current directory to the newly created one:
```
cd Todo
```
Next,the command **npm init** is run to initialise the project to create a new file named package.json.
```bash
npm init
```

THe package.json creation is confirmed as shown [here](https://user-images.githubusercontent.com/61512079/173153396-5c10886e-8cc4-433b-88ca-ec3f282d1322.PNG)

Next, we will Install ExpressJs and create the Routes directory:
```bash
npm install express
```
ExpressJS installation is confirmed [here](https://user-images.githubusercontent.com/61512079/173154270-c912fc7f-c98f-4803-9fb5-a961ac352428.PNG)

A file index.js was with the command below:
```bash
touch index.js
```
Install the **dotenv** module with the command below:
```bash
npm install dotenv
```
Next the index.js is edited with vim command and the set of codes below is pasted in it:
```bash
vi index.js
```
```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```
Next we run node command on the index.js file:
```bash
node index.js
```
The server is confirm running on port [5000](https://user-images.githubusercontent.com/61512079/173155141-0769af53-1236-4034-8bbe-8f21d3291a1b.PNG)

Next we allowed port 5000 in the [inbound rule](https://user-images.githubusercontent.com/61512079/173155739-f5709a8a-a625-42fc-a7a8-7295aa4ea529.PNG)
 of our EC2 instance to enable web access on port 5000.
This is confirmed on the browser [here.](https://user-images.githubusercontent.com/61512079/173155948-380bc88c-cb4a-46c7-9e05-73e83bd5e452.PNG)

Next we created Routes for the three actions we want our ToDO Application to carry out:
1. Create a new task
1. Display list of all tasks
1. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: **POST, GET, DELETE.**

We created a directory for the route as follow:
```bash
mkdir route
```
Change directory to routes folder:
```bash
cd routes
```

Now, create a file api.js with the command below:
```bash
touch api.js
```
Next we copy the code below into api.js file.
```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

Since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model and a schema.
To create a Schema and a model, install **mongoose** which is a Node.js package that makes working with mongodb easier. 
Change directory back to Todo director and Install it with the command below:
```bash
   cd..
   npm install mongoose
```
Create a new folder models :
```bash
mkdir models
```
Change directory into the newly created ‘models’ folder with
```
cd models
```
Inside the models folder, create a file and name it todo.js
```
touch todo.js
```
Open the file created with vim todo.js then paste the code below in the file:
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit

```

Next we created a MongoDB database using MLab DBaaS.  Steps below to Sign up on MLab:

Carry out all the steps for creating a cluster and database in the MLab.

Generate the connection string with the password and add it to the .env file.


Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.  Just delete entries in the file and replace with the code below:
```
Start your server using the command:
```
node index.js
```
The databased was successfully connected as shown in the [log.](https://user-images.githubusercontent.com/61512079/173161350-8169a4b3-d5a9-4068-82fb-d24a671d3faf.PNG)

**Testing Backend Code without Frontend using RESTful API**
So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that.
In this project, we will use Postman to test our API.  DOwnload  Postman on the system.

Below is the result of the POSTMAN CRUD action: POST,GET, DELETE
[POST](https://user-images.githubusercontent.com/61512079/173164983-f5c8ca1d-6e8d-47f0-8abc-36b2a9bdbeea.PNG)
[GET](https://user-images.githubusercontent.com/61512079/173165010-fa1b8f9d-fbdc-4604-831a-51665927e22e.PNG)
[DELETE](https://user-images.githubusercontent.com/61512079/173165034-8ea47d86-e806-4a48-a6f7-55795ec1196e.PNG)

FRONT-END APPLICATION
In the same root directory as your backend code, which is the Todo directory, run:

 ```bash
 npx create-react-app client
 ```
 Install React dependencies:
1. Install concurrently. 
```bash
npm install concurrently --save-dev
```
2. Install nodemon. 
```bash
npm install nodemon --save-dev
```

Edit the content of package.json with the code below:
```bash
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

Configure Proxy in package.json as follow:

1. Change directory to ‘client’
```bash
cd client
```

2. Open the package.json file
```bash
vi package.json
```
3. Add the key value pair in the package.json file 
```javascript
"proxy": "http://localhost:5000"
```

Now, ensure you are inside the Todo directory, and simply do:
```bash
npm run dev
```

Creating your React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.

From your Todo directory run
```bash
cd client
```

move to the src directory

```bash
cd src
```
Inside your src folder create another folder called components

mkdir components
Move into the components directory with
```bash
cd components
```
Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

```bash
touch Input.js ListTodo.js Todo.js
```
Open Input.js file
```bash
vi Input.js
```

Copy and paste the following
```bash
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
```
To make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the src folder
```bash
cd ..
```

Move to clients folder
```bash
cd ..
Install Axios
```
```bash
npm install axios
```

Go to ‘components’ directory
```bash
cd src/components'
```
After that open your ListTodo.js
```bash
vi ListTodo.js
```
Copy and paste the code below into ListTodo.js
```javascript
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
Then in your Todo.js file you write the following code

import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
```
In the src directory open the index.css
```bash
vim index.css
```

Copy and paste the code below:

```css
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```
Run the React web app using the command below:

```bash
npm run dev
```

The app should run without error as show in [react-status](https://user-images.githubusercontent.com/61512079/173194299-070bc689-4b2e-4ac7-bd50-337458c64cda.PNG)
 here.

From the browser, the app is tested by opening http://ec2-18-190-154-2.us-east-2.compute.amazonaws.com:3000 ., the result should opened as shown [here.](https://user-images.githubusercontent.com/61512079/173855340-848851f3-d3aa-4a6d-87a5-cc0c8a6dc9df.PNG)

