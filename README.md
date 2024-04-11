# Create a Fullstack application with ReactJS + NodeJS + MongoDB (local) 

####  Create React App
```bash
npx create-react-app mongoapp
cd mongoapp
npm start 
```
#### Create a Node Backend
```bash
mkdir backend
cd backend
npm install mongodb cors
```
#### Start your MongoDB Server
ubuntu
```bash
sudo systemctl start mongod
sudo systemctl status mongod
```
optional
```bash
mongosh
```
Database Name: `ipLab` <br>
Collection Name: `credential` <br>
#### Create a new file `backend/server.js`
```js
// server.js
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const { MongoClient } = require('mongodb');

const app = express();
const PORT = process.env.PORT || 5000;

// Connection URI
const uri = "mongodb://localhost:27017";
const dbName = "ipLab";

// Middleware
app.use(cors()); // Enable CORS
app.use(bodyParser.json());

// Connect to MongoDB
async function connect() {
  try {
    // Create a new MongoClient instance
    const client = new MongoClient(uri);
    
    // Connect to MongoDB
    await client.connect();
    console.log("Connected to MongoDB server");

    // Access the database
    const db = client.db(dbName);

      const collectionName = "credential";
    // Define collection
    const usersCollection = db.collection(collectionName);
      
    console.log("Connected to MongoDB server\nDatabase: " + dbName + " Collection: " + collectionName);

    // Signup endpoint
    app.post('/signup', async (req, res) => {
      const { username, password } = req.body;

      try {
        // Check if the username already exists
        const existingUser = await usersCollection.findOne({ username });
        if (existingUser) {
          return res.status(400).json({ message: 'Username already exists' });
        }

        // Insert the new user into the collection
        const newUser = await usersCollection.insertOne({ username, password });

        res.status(201).json({ message: 'Signup successful'});
      } catch (err) {
        console.error("Error signing up:");
        res.status(500).json({ message: 'Internal server error' });
      }
    });

    // Login endpoint
    app.post('/login', async (req, res) => {
      const { username, password } = req.body;

      try {
        // Find user by username
        const user = await usersCollection.findOne({ username });

        if (!user) {
          return res.status(404).json({ message: 'User not found' });
        }

        // Check password
        if (user.password !== password) {
          return res.status(401).json({ message: 'Invalid password' });
        }

        // Return a success message or token if needed
        res.json({ message: 'Login successful', user });
      } catch (err) {
        console.error("Error logging in:");
        res.status(500).json({ message: 'Internal server error' });
      }
    });

    // Start server
    app.listen(PORT, () => {
      console.log(`Server is running on port ${PORT}`);
    });
  } catch (err) {
    console.error("Error connecting to MongoDB:", err);
  }
}

// Call the connect function
connect();
```
#### `mongoapp/src/App.js`
```js
import { useState } from 'react';

function App() {

    const [user, setUser] = useState('');
    const [password, setPassword] = useState('');

    function callLogin() {
        const postData = {
        username: user,
        password: password
        };

        fetch('http://localhost:5000/login', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(postData)
        })
        .then(response => response.json())
        .then(data => console.log(data))
        .catch(error => console.error('Error:', error));

    }

    function callSignup() {
        const postData = {
        username: user,
        password: password
        };

        fetch('http://localhost:5000/signup', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(postData)
        })
        .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
        })
        .then(data => {
        console.log('Signup successful:', data);
        })
        .catch(error => {
        console.error('Error:', error);
        });

    }

    function updateUser(event) {
        setUser(event.target.value);
    }

    function updatePass(event) {
        setPassword(event.target.value);
    }

  const formStyle = {
    padding: '50px',
    backgroundColor: 'red',
    
  }
  const AppStyle = {
  textAlign: "center",
  display: "flex",
  justifyContent: "center",
    alignContent: "center",
  marginTop:"5%",
  }
  
  return (
    <div style={AppStyle}>
      <table style={formStyle} >
        <tr>
          <th><label>UserName </label></th>
          <th> <input type="text" placeholder="username" value={user} onChange={updateUser}></input> </th>
        </tr>  
        <tr>
          <th><label>Password </label></th>
          <th><input type="password" placeholder="password" value={password} onChange={updatePass}></input></th>
        </tr>
        <tr>
          <td><button onClick={callLogin}>Login</button></td>
          <td><button onClick={callSignup}>Register New User</button></td>
        </tr>
      </table>        
    </div>
  );
}

export default App;
```
### Start the Backed or Node server.
```bash
node server.js
```
### Start the react frontend.
```bash
npm start
```

# Thank you....
