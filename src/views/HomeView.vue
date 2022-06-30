<template>
  <div class="home">
    <div class="row">
      <label for="courses">List of Courses: </label>
      <select id="courses" @change="selectCourse($event)" name="courses" >
        <option value="" disabled selected>Select Course</option>
        <option v-for="course in courses" :value="course.id">{{course.name}}</option>
      </select>
      <label for="students">List of Students: </label>
      <select id="students" @change="selectStudent($event)" name="students">
        <option value="" disabled selected>Select Student</option>
        <option v-for="student in students" :value="student.id">{{student.first_name}} {{student.last_name}}</option>
      </select>
      <label for="teachers">List of Teachers: </label>
      <select id="teachers" @change="selectTeacher($event)" name="teachers">
        <option value="" disabled selected>Select Teacher</option>
        <option v-for="teacher in teachers" :value="teacher.id">{{teacher.first_name}} {{teacher.last_name}}</option>
      </select>
    </div>
    <h5>Teacher(s)</h5>
    <div v-for="teacher in courses_teachers">
      <p>{{teacher.first_name}} {{teacher.last_name}}</p>
    </div>
    <h5>Students</h5>
    <div v-for="student in courses_students">
      <p>{{student.first_name}} {{student.last_name}}</p>
    </div>
    <h5>Courses</h5>
    <div v-for="course in students_courses">
      <p>{{course.name}}</p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  data: function() {
    return {
      courses: [],
      students: [],
      teachers: [],
      courses_students: [],
      courses_teachers: [],
      teacher_courses: [],
      teacher_students: [],
      students_courses: [],
      students_teachers: [],

      currentCourse: {},
      currentStudent: {},
      currentTeacher: {},

    };
  },

  created: function() {
    axios.get("/api/courses").then(response => {
      this.courses = response.data;
    });
    axios.get("/api/students").then(response => {
      this.students = response.data;
    });
    axios.get("/api/teachers").then(response => {
      this.teachers = response.data;
    });
  },

  methods: {

    selectCourse: function(theCourse) {
      console.log('selecting the course...', theCourse.target.value);
      if (theCourse.target.value) {
        const id = parseInt(theCourse.target.value, 10);
        axios.get(`/api/courses/${id}`).then(response => {
          console.log('course details ...', response.data);
          if (response.data) {
            this.students_courses = [];
            this.currentCourse = response.data;
            if (response.data.students && response.data.students.length) {
              this.courses_students = response.data.students;
            }
            if (response.data.teachers && response.data.teachers.length) {
              this.courses_teachers = response.data.teachers;
            }
          }
        });
      }
    },

    selectStudent: function(theStudent) {
      console.log('selecting the student...', theStudent.target.value);
      if (theStudent.target.value) {
        const id = parseInt(theStudent.target.value, 10);
        axios.get(`/api/students/${id}`).then(response => {
          console.log('student details ...', response.data);
          if (response.data) {
            this.courses_teachers = [];
            this.courses_students = [];
            this.currentStudent = response.data;
            if (response.data.courses && response.data.courses.length) {
              this.students_courses = response.data.courses;
            }
          }
        });
      }
    },

    selectTeacher: function(theTeacher) {
      console.log('selecting the teacher...', theTeacher.target.value);
      if (theTeacher.target.value) {
        const id = parseInt(theTeacher.target.value, 10);
        axios.get(`/api/teachers/${id}`).then(response => {
          console.log('teacher details ...', response.data);
          if (response.data) {
            this.courses_students = [];
            this.courses_teachers = [];
            this.currentTeacher = response.data;
            if (response.data.courses && response.data.courses.length) {
              this.students_courses = response.data.courses;
            }

          }
        });
      }
    },

    toggleCourse: function(theCourse) {
      console.log(theCourse);
      if (this.currentCourse === theCourse) {
        this.currentCourse = null;
      } else {
        this.currentCourse = theCourse;
      }
      console.log('in toggle info...');
    },

  }
}
</script>
