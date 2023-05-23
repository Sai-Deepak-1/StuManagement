# StuManagement
Student Management System

https://github.com/Sai-Deepak-1/StuManagement.git

Sure! Here's a step-by-step procedure to create a student management system using Node.js, Angular, and MongoDB:

Step 1: Set up the development environment
- Install Node.js and npm (Node Package Manager) on your system.
- Install Angular CLI (Command Line Interface) globally by running the following command in the terminal:
  ```
  npm install -g @angular/cli
  ```
- Install MongoDB and make sure it's running on your system.

Step 2: Create a new Angular project
- Open a terminal and navigate to the directory where you want to create your project.
- Run the following command to create a new Angular project:
  ```
  ng new student-management-system
  ```
- Change to the project directory:
  ```
  cd student-management-system
  ```

Step 3: Create the backend using Node.js and Express
- In the root directory of your project, create a new directory called `server`.
- Navigate to the `server` directory:
  ```
  cd server
  ```
- Initialize a new Node.js project:
  ```
  npm init -y
  ```
- Install the required dependencies:
  ```
  npm install express mongoose body-parser cors
  ```
- Create a file called `server.js` and set up the basic Express server with routes for CRUD operations. You can refer to the Express documentation for more details on how to set up routes and interact with MongoDB using Mongoose.

Step 4: Create the frontend using Angular
- Open a new terminal and navigate back to the root directory of your Angular project.
- Generate a new component for the student management system:
  ```
  ng generate component student-management
  ```
- Open the generated component file `student-management.component.ts` and define properties and methods for handling form inputs and CRUD operations.

Step 5: Create a form for input and display the output
- Open the generated HTML file `student-management.component.html` and create a form using Angular's form directives. Include input fields for the student's details (e.g., name, age, grade, etc.).
- Add buttons or links to perform CRUD operations (Create, Read, Update, Delete) and bind them to the corresponding methods in the component.

Step 6: Connect the frontend to the backend
- In the `student-management.component.ts` file, make HTTP requests to the backend API using Angular's `HttpClient` module. Use the appropriate HTTP methods (POST, GET, PUT, DELETE) to perform CRUD operations.
- Update the component methods to handle the responses from the backend and update the UI accordingly.

Step 7: Test and run the application
- Open a terminal and navigate to the root directory of your Angular project.
- Start the Angular development server:
  ```
  ng serve
  ```
- In another terminal, navigate to the `server` directory and start the Node.js server:
  ```
  node server.js
  ```

That's it! You've now created a student management system using Node.js, Angular, and MongoDB, capable of performing CRUD operations. You can further enhance the system by adding authentication, validation, and additional features as per your requirements.



Certainly! Here's an example of the code for the student management system with some minimal styling:

First, let's create the backend using Node.js and Express. In the `server.js` file:

```javascript
// server.js

const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = 3000;

// Middleware
app.use(cors());
app.use(bodyParser.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost/studentDB', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Define the student schema
const studentSchema = new mongoose.Schema({
  name: String,
  age: Number,
  grade: String,
});

// Define the student model
const Student = mongoose.model('Student', studentSchema);

// Routes
app.get('/api/students', (req, res) => {
  // Retrieve all students
  Student.find({}, (err, students) => {
    if (err) {
      res.status(500).send(err);
    } else {
      res.status(200).send(students);
    }
  });
});

app.post('/api/students', (req, res) => {
  // Create a new student
  const newStudent = new Student({
    name: req.body.name,
    age: req.body.age,
    grade: req.body.grade,
  });

  newStudent.save((err) => {
    if (err) {
      res.status(500).send(err);
    } else {
      res.status(201).send('Student created successfully');
    }
  });
});

// ... Add routes for updating and deleting students ...

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

Now, let's create the frontend using Angular. In the `student-management.component.html` file:

```html
<!-- student-management.component.html -->

<div class="container">
  <h2>Add Student</h2>
  <form (submit)="addStudent()">
    <div class="form-group">
      <label for="name">Name:</label>
      <input type="text" class="form-control" id="name" [(ngModel)]="newStudent.name" name="name" required>
    </div>
    <div class="form-group">
      <label for="age">Age:</label>
      <input type="number" class="form-control" id="age" [(ngModel)]="newStudent.age" name="age" required>
    </div>
    <div class="form-group">
      <label for="grade">Grade:</label>
      <input type="text" class="form-control" id="grade" [(ngModel)]="newStudent.grade" name="grade" required>
    </div>
    <button type="submit" class="btn btn-primary">Add</button>
  </form>

  <hr>

  <h2>Student List</h2>
  <table class="table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Age</th>
        <th>Grade</th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let student of students">
        <td>{{ student.name }}</td>
        <td>{{ student.age }}</td>
        <td>{{ student.grade }}</td>
      </tr>
    </tbody>
  </table>
</div>
```

In the `student-management.component.ts` file:

```typescript
// student-management.component.ts

import { Component, OnInit } from '@angular/core';
import { HttpClient

 } from '@angular/common/http';

interface Student {
  name: string;
  age: number;
  grade: string;
}

@Component({
  selector: 'app-student-management',
  templateUrl: './student-management.component.html',
  styleUrls: ['./student-management.component.css']
})
export class StudentManagementComponent implements OnInit {
  students: Student[] = [];
  newStudent: Student = { name: '', age: 0, grade: '' };

  constructor(private http: HttpClient) { }

  ngOnInit(): void {
    this.getStudents();
  }

  getStudents(): void {
    this.http.get<Student[]>('/api/students').subscribe((students) => {
      this.students = students;
    });
  }

  addStudent(): void {
    this.http.post('/api/students', this.newStudent).subscribe(() => {
      this.newStudent = { name: '', age: 0, grade: '' };
      this.getStudents();
    });
  }
}
```

In the `styles.css` file:

```css
/* styles.css */

.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.form-group {
  margin-bottom: 10px;
}

.btn {
  margin-top: 10px;
}

table {
  width: 100%;
  margin-top: 20px;
}

th, td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}
```

Remember to include the necessary Angular and HttpClientModule imports in the `app.module.ts` file.

With these files in place, you should be able to run the application by starting the Angular development server with `ng serve` and the Node.js server with `node server.js`. You can then access the student management system in your browser at `http://localhost:4200`.

Please note that this is a basic implementation, and you can further enhance and style the application according to your requirements.
