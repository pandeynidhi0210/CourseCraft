import React, { useEffect, useState } from 'react';
import Course from './Course';
import base_url from '../api/bootapi';
import axios from 'axios';
import { toast } from 'react-toastify';

const Allcourses = () => {
  const [courses, setCourses] = useState([]);

  useEffect(() => {
    document.title = "All Courses";
    getAllCourses();
  }, []);

  const getAllCourses = () => {
    axios.get(`${base_url}/courses`).then(
      (response) => {
        setCourses(response.data);
      },
      (error) => {
        toast.error("Failed to load courses");
      }
    );
  };

  const removeCourseById = (id) => {
    setCourses(courses.filter(c => c.id !== id));
  };

  return (
    <div>
      <h2 className='text-center'>All Courses</h2>
      <p className='text-center'>List of courses are as follows:</p>
      {
        courses.length > 0 ? 
          courses.map(course => (
            <Course key={course.id} course={course} update={removeCourseById} />
          )) : 
          <p>No courses available</p>
      }
    </div>
  );
};

export default Allcourses;
/////////////////////////////////////////////////////////
import React, { useEffect, useState } from 'react';
import { Form, FormGroup, Label, Input } from 'reactstrap';
import Container from '@mui/material/Container';
import { Button } from '@mui/material';
import axios from 'axios';
import base_url from '../api/bootapi';
import { toast } from 'react-toastify';

const Conclude = () => {
  const [isEdit, setIsEdit] = useState(false);
  const [course, setCourse] = useState({
    id: '',
    title: '',
    description: ''
  });

  useEffect(() => {
    document.title = isEdit ? "Update Course" : "Add Course";
    const storedCourse = localStorage.getItem('editCourse');
    if (storedCourse) {
      setCourse(JSON.parse(storedCourse));
      setIsEdit(true);
      localStorage.removeItem('editCourse');
    }
  }, []);

  const handleForm = (e) => {
    e.preventDefault();

    if (!course.id || !course.title || !course.description) {
      toast.error("All fields are required!");
      return;
    }

    if (isEdit) {
      updateCourse(course);
    } else {
      addCourse(course);
    }
  };

  const addCourse = (data) => {
    axios.post(`${base_url}/courses`, data).then(
      () => {
        toast.success("Course added successfully");
        clearForm();
      },
      () => {
        toast.error("Failed to add course");
      }
    );
  };

  const updateCourse = (data) => {
    axios.put(`${base_url}/courses/${data.id}`, data).then(
      () => {
        toast.success("Course updated successfully");
        clearForm();
        setIsEdit(false);
      },
      () => {
        toast.error("Update failed");
      }
    );
  };

  const clearForm = () => {
    setCourse({ id: '', title: '', description: '' });
    setIsEdit(false);
  };

  return (
    <Container>
      <h1 className="text-center my-3">{isEdit ? "Update Course" : "Add Course"}</h1>
      <Form onSubmit={handleForm}>
        <FormGroup>
          <Label for="id">Course Id</Label>
          <Input
            type="text"
            name="id"
            id="id"
            placeholder="Enter Course ID"
            value={course.id}
            onChange={(e) => setCourse({ ...course, id: e.target.value })}
            disabled={isEdit}
          />
        </FormGroup>
        <FormGroup>
          <Label for="title">Course Title</Label>
          <Input
            type="text"
            name="title"
            id="title"
            placeholder="Enter Title"
            value={course.title}
            onChange={(e) => setCourse({ ...course, title: e.target.value })}
          />
        </FormGroup>
        <FormGroup>
          <Label for="description">Course Description</Label>
          <Input
            type="textarea"
            name="description"
            id="description"
            placeholder="Enter Description"
            value={course.description}
            onChange={(e) => setCourse({ ...course, description: e.target.value })}
          />
        </FormGroup>
        <Button type="submit" color="success" className="me-2">
          {isEdit ? "Update" : "Add"} Course
        </Button>
        <Button type="button" color="warning" onClick={clearForm}>
          Clear
        </Button>
      </Form>
    </Container>
  );
};

export default Conclude;
///////////////////////////////////////////////////////////
import React from 'react';
import { Button, Card, CardBody, CardSubtitle, CardText, CardTitle } from 'reactstrap';
import axios from 'axios';
import base_url from '../api/bootapi';
import { toast } from 'react-toastify';

const Course = ({ course, update }) => {
  
  const deleteCourse = (id) => {
    axios.delete(`${base_url}/courses/${id}`).then(
      (response) => {
        toast.success("Course deleted");
        update(id); // inform parent to update state
      },
      (error) => {
        toast.error("Delete failed");
      }
    );
  };

  const handleUpdate = () => {
    localStorage.setItem('editCourse', JSON.stringify(course));
    window.location.href = "/add-course"; // navigate to update form
  };

  return (
    <Card className='text-center'>
      <CardBody>
        <CardTitle className='fw-bold'>{course.title}</CardTitle>
        <CardSubtitle className='mb-2 text-muted'>ID: {course.id}</CardSubtitle>
        <CardText>{course.description}</CardText>
        <Button color='danger' onClick={() => deleteCourse(course.id)} className='me-2'>Delete</Button>
        <Button color='warning' onClick={handleUpdate}>Update</Button>
      </CardBody>
    </Card>
  );
};

export default Course;
///////////////////////////////////////////////////////
import React from 'react';
import {Card,CardBody} from "reactstrap"; 

function Header({name,title})
{
    return (
        <div>
            <Card className="my-2 bg-warning">
               <CardBody>
               <h1 className="text-center my-2">Welcome to Course Application</h1>
                </CardBody> 
            </Card>
            
        </div>
    )
}
export default Header;
//////////////////////////////////////////////////////
import React ,{useEffect} from 'react';
import { Jumbotron,Container,Button} from "reactstrap";
const Home = () =>
{
    useEffect(()=>{document.title="Home || Learn code with nidhi"},[]);
    return (
        <div>
            <Jumbotron className=" text-center " >
               
                <h1 >Learn Code With Nidhi</h1>
                <p>This is developed by Learn Code With Nidhi for learning purpose .Its Backend is on spring boot and frontend on react js</p>
                <Container>
                    <Button color="primary" outline >
                        Start Using
                    </Button>
                </Container>
            </Jumbotron>
        </div>
    )
};
export default Home;
/////////////////////////////////////////////////
import React from "react";
import {ListGroup,ListGroupItem} from "reactstrap";
import { Link } from 'react-router-dom';
const Menus=()=>{
    return (
        <ListGroup>
            <Link className="list-group-item list-group-item-action" tag="a" to="/" action >
                Home
            </Link >
            <Link className="list-group-item list-group-item-action"tag="a" to="/Conclude" action >
                Add Course
            </Link>
            <Link className="list-group-item list-group-item-action" tag="a" to="/view-courses" action >
                View Courses
            </Link>
            <Link className="list-group-item list-group-item-action" tag="a" to="#!" action >
                About Us
            </Link>
            <Link className="list-group-item list-group-item-action" tag="a" to="#!" action >
            Contact
            </Link>
        </ListGroup>

    );
};
export default Menus;
////////////////////////////////////////////////
import React from 'react';
import 'bootstrap/dist/css/bootstrap.min.css';
import { ToastContainer, toast } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';
import Home from './components/Home';
import Course from './components/Course';
import Allcourses  from "./components/Allcourses";
import Conclude from "./components/Conclude";
import {Button,Container,Row,Col} from "reactstrap";
import Header from "./components/Header";
import Menus from"./components/Menus";
import { BrowserRouter, Routes, Route} from 'react-router-dom';
function App() {
    return (
      <div className="app-container">
        
        <BrowserRouter>
          <ToastContainer />
          <Container>
            <Header />
            <Row>
              <Col md={4}>
                <Menus /> {/* Assuming Menus is a component */}
              </Col>
              <Col md={8}>
                <Routes>
                  <Route path="/" element={<Home />} />
                  <Route path="/conclude" element={<Conclude />} />
                  <Route path="/view-courses" element={<Allcourses />} />
                </Routes>
              </Col>
            </Row>
          </Container>
        </BrowserRouter>
      </div>
  );
}

export default App;

////////////////////////////////////////////////////////////////
import React from 'react';
import './index.css';

import { createRoot } from 'react-dom/client';
import App from './App';
import 'bootstrap/dist/css/bootstrap.min.css';
import 'react-toastify/dist/ReactToastify.css';
const container = document.getElementById('root');
const root = createRoot(container);
root.render(<App />);



//////////////////////////////////////////////////////////////
