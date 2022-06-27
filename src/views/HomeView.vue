<template>
  <div class="home">
    <div class="row">
      <label for="courses">Choose a Course: </label>
      <select id="courses" name="courses">
          <option v-for="course in courses">{{course.name}}</option>
      </select>
      <label for="students">Choose a Student: </label>
      <select id="students" name="students">
          <option v-for="student in students" :value= "student.id">{{student.first_name}} {{student.last_name}}</option>
      </select>
      <label for="teachers">Choose a Teacher: </label>
      <select id="teachers" name="teachers">
          <option v-for="teacher in teachers">{{teacher.first_name}} {{teacher.last_name}}</option>
      </select>
    </div>
    <div class="row">

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

  }
}
</script>
