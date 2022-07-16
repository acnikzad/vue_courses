<template>
  <div class="home">
    <!-- Courses section-->
    <section class="py-3 border-bottom" id="features">
            <div class="container px-5 my-5">
                <div class="row gx-5">
<!-- Add Student -->
                    <div class="col-lg-3 mb-5 mb-lg-0">
                      <div class="text-center mb-5">
                        <h2 class="fw-bolder">Add Student</h2>
                      </div>
                        <form id="studentForm">
                            <!-- Name input-->
                            <div class="form-floating mb-3">
                                <input class="form-control" id="name" type="text" placeholder="Enter your name..." data-sb-validations="required" v-model="newStudentFirstName"/>
                                <label for="name">First Name</label>
                                <div class="invalid-feedback" data-sb-feedback="name:required">A name is required.</div>
                            </div>
                            <!-- Email address input-->
                            <div class="form-floating mb-3">
                                <input class="form-control" id="email" type="text" placeholder="name@example.com" data-sb-validations="required" v-model="newStudentLastName"/>
                                <label for="email">Last Name</label>
                            </div>
                            <!-- Submit Button-->
                            <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" v-on:click="createStudent()">Submit</button></div>
                        </form>
                    </div>
<!-- Add Teacher -->
                    <div class="col-lg-3 mb-5 mb-lg-0">
                      <div class="text-center mb-5">
                        <h2 class="fw-bolder">Add Teacher</h2>
                      </div>
                        <form id="teacherForm">
                            <!-- Name input-->
                            <div class="form-floating mb-3">
                                <input class="form-control" id="name" type="text" placeholder="Enter your name..." data-sb-validations="required" v-model="newTeacherFirstName"/>
                                <label for="name">First Name</label>
                                <div class="invalid-feedback" data-sb-feedback="name:required">A name is required.</div>
                            </div>
                            <!-- Email address input-->
                            <div class="form-floating mb-3">
                                <input class="form-control" id="email" type="text" placeholder="name@example.com" data-sb-validations="required" v-model="newTeacherLastName"/>
                                <label for="email">Last Name</label>
                            </div>
                            <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" v-on:click="createTeacher()">Submit</button></div>
                        </form>
                    </div>
<!-- Assign Student -->
                    <div class="col-lg-3 mb-5 mb-lg-0">
                        <div class="text-center mb-5">
                          <h2 class="fw-bolder">Assign Classes to Students</h2>
                        </div>
                        <form name="assignStudent" @submit.prevent="assignStudent">
                          <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" v-model="studentID">
                            <option value="" disabled selected>Select Student</option>
                            <option id="theStudent" v-for="student in students" :value="student.id">{{student.first_name}} {{student.last_name}}</option>
                          </select>
                          <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" v-model="studentcourseID">
                            <option value="" disabled selected>Select Course</option>
                            <option id="theCourse" v-for="course in courses" :value="course.id">{{course.name}}</option>
                          </select>     
                          <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" value="Submit" v-on:click="assignStudent()">Submit</button>
                        </div>
                      </form>
                    </div>
<!-- Assign Teacher -->
                    <div class="col-lg-3 mb-5 mb-lg-0">
                        <div class="text-center mb-5">
                          <h2 class="fw-bolder">Assign Classes to Teachers</h2>
                        </div>
                        <form name="assignTeacher" @submit.prevent="assignTeacher">
                          <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" v-model="teacherID">
                            <option value="" disabled selected>Select Teacher</option>
                            <option id="theTeacher" v-for="teacher in teachers" :value="teacher.id">{{teacher.first_name}} {{teacher.last_name}}</option>
                          </select>
                          <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" v-model="teachercourseID">
                            <option value="" disabled selected>Select Course</option>
                            <option id="theCourse" v-for="course in courses" :value="course.id">{{course.name}}</option>
                          </select>
                          <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" value="Submit" v-on:click="assignTeacher()">Submit</button>
                        </div>
                      </form>
                    </div>
                </div>
            </div>
        </section>
<!-- Display -->
        <section class="bg-light py-5 border-bottom">
          <div class="container px-5 my-5">
            <div class="container px-5 my-5">
                <div class="row gx-5">
                    <div class="col-lg-4 mb-5 mb-lg-0 large text-uppercase fw-bold text-muted">
                      <div class="text-center mb-5">
                        <h4 class="fw-bolder">Select Class for Enrollment</h4>
                      </div>
                      <select id="courses" @change="selectCourse($event)" name="courses" class="form-select form-select-lg mb-3" aria-label=".form-select-lg example">
                        <option value="" disabled selected>Select Course</option>
                        <option v-for="course in courses" :value="course.id">{{course.name}}</option>
                      </select>
                    </div>
                    <div class="col-lg-4 mb-5 mb-lg-0 large text-uppercase fw-bold text-muted">
                      <div class="text-center mb-5">
                        <h4 class="fw-bolder">Select Student for Assigned Classes</h4>
                      </div>
                      <select id="students" @change="selectStudent($event)" name="students" class="form-select form-select-lg mb-3" aria-label=".form-select-lg example">
                        <option value="" disabled selected>Select Student</option>
                        <option v-for="student in students" :value="student.id">{{student.first_name}} {{student.last_name}}</option>
                      </select>
                    </div>
                    <div class="col-lg-4 mb-5 mb-lg-0 large text-uppercase fw-bold text-muted">
                      <div class="text-center mb-5">
                        <h4 class="fw-bolder">Select Teacher for Assigned Classes</h4>
                      </div>
                      <select id="teachers" @change="selectTeacher($event)" name="teachers" class="form-select form-select-lg mb-3" aria-label=".form-select-lg example">
                        <option value="" disabled selected>Select Teacher</option>
                        <option v-for="teacher in teachers" :value="teacher.id">{{teacher.first_name}} {{teacher.last_name}}</option>
                      </select>
                    </div>
                </div>
            </div>
            <div class="row gx-5 justify-content-center">
              <div class="col-lg-6 col-xl-4">
                <div class="card mb-5 mb-xl-0">
                  <div class="card-body p-5">
                    <div class="large text-uppercase fw-bold text-muted">Courses</div>
                    <div v-for="course in students_courses">
                      <p>{{course.name}}</p>
                    </div>
                  </div>
                </div>
              </div>
              <div class="col-lg-6 col-xl-4">
                <div class="card mb-5 mb-xl-0">
                  <div class="card-body p-5">
                    <div class="large text-uppercase fw-bold text-muted">Students</div>
                    <div v-for="student in courses_students">
                      <p>{{student.first_name}} {{student.last_name}}</p>
                    </div>
                  </div>
                </div>
              </div>
              <div class="col-lg-6 col-xl-4">
                <div class="card mb-5 mb-xl-0">
                  <div class="card-body p-5">
                    <div class="large text-uppercase fw-bold text-muted">Teachers</div>
                      <div v-for="teacher in courses_teachers">
                        <p>{{teacher.first_name}} {{teacher.last_name}}</p>
                      </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>
        <section class="py-3 border-bottom" id="features">
            <div class="container px-5 my-5">
                <div class="row gx-5">
<!-- Remove Student -->
                    <div class="col-lg-6 mb-5 mb-lg-0">
                        <div class="text-center mb-5">
                          <h2 class="fw-bolder">Remove Student</h2>
                        </div>
                        <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" @change="destroyStudent($event)">
                          <option value="" disabled selected>Select Student</option>
                          <option v-for="student in students" :value="student.id">{{student.first_name}} {{student.last_name}}</option>
                        </select>
                        <!-- <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" v-on:click="destroyStudent($event)">Remove</button></div> -->
                    </div>
<!-- Remove Teacher -->
                    <div class="col-lg-6 mb-5 mb-lg-0">
                        <div class="text-center mb-5">
                          <h2 class="fw-bolder">Remove Teacher</h2>
                        </div>
                        <form name="assignTeacher" onsubmit="assignTeacher()">
                          <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example">
                            <option value="" disabled selected>Select Teacher</option>
                            <option id="theTeacher" v-for="teacher in teachers" :value="teacher.id">{{teacher.first_name}} {{teacher.last_name}}</option>
                          </select>
                          <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" v-on:click="destroyTeacher($event)" value="Submit">Remove</button>
                        </div>
                      </form>
                    </div>
                </div>
            </div>
        </section>
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
      selected_student: [],

      teacherID: "",
      studentID: "",
      studentcourseID: "",
      teachercourseID: "",

      currentCourse: {},
      currentStudent: {},
      currentTeacher: {},

      // event: [],

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
      console.log('selecting the course...', theCourse);
      if (theCourse.target.value) {
        const id = parseInt(theCourse.target.value, 10);
        axios.get(`/api/courses/${id}`).then(response => {
          console.log('course details ...', response.data);
          if (response.data) {
            this.courses_teachers = [];
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
            this.students_courses = [];
            this.courses_students = [];
            this.courses_teachers = [];
            this.currentStudent = response.data;
            if (response.data.courses && response.data.courses.length) {
              this.students_courses = response.data.courses;
              // this.courses_students = response.data.students;
            }
            if (response.data.students && response.data.students.length) {
              this.courses_students = response.data.students;
              // this.courses_students = response.data.students;
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

    createStudent: function() {
      console.log("creating student...")
      let params = {
            first_name: this.newStudentFirstName,
            last_name: this.newStudentLastName
      };
      axios.post("/api/students", params).then(response => {
        this.students.push(response.data);
        this.newStudentFirstName = "";
        this.newStudentLastName = "";
      })
    },

    createTeacher: function() {
      console.log("creating teacher...")
      let params = {
            first_name: this.newTeacherFirstName,
            last_name: this.newTeacherLastName
      };
      axios.post("/api/teachers", params).then(response => {
        this.teachers.push(response.data);
        this.newTeacherFirstName = "";
        this.newTeacherLastName = "";
      })
    },

    assignStudent: function(event) {
      event.preventDefault();
      const {studentID, studentcourseID } = this;
      console.log('assigning the student...', studentID, studentcourseID);
    },

    assignTeacher: function(event) {
      event.preventDefault();
      const {teacherID, teachercourseID } = this;
      console.log('assigning the teacher...', teacherID, teachercourseID);
    },

    destroyStudent: function(student) {
      console.log(student.target.value)
      axios.delete("/api/students/" + student.target.value).then(response => {
        let index = this.students.indexOf(student.target.value);
        this.students.splice(index, 1);
      })

    },

    destroyTeacher: function() {

    }
  }
}
</script>
