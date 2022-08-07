<template>
  <div id="app" class="home">
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
                          <input class="form-control" id="grade" type="number" min="1" max="100" placeholder="Enter Percentage" v-model="grade" />
                          <br> 
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
                <div class="col-lg-3 col-xl-3">
                  <div class="card mb-5 mb-xl-0">
                    <div class="card-body p-5">
                      <table class="table">
                        <thead>
                          <tr>
                            <th scope="col">Class</th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr v-for="course in students_courses">
                            <th scope="row">{{course.name}}</th>
                          </tr>
                        </tbody>
                      </table>
                  </div>
                </div>
              </div>
              <div class="col-lg-3 col-xl-3">
                <div class="card mb-5 mb-xl-0">
                  <div class="card-body p-5">
                    <table class="table">
                      <thead>
                        <tr>
                          <th scope="col">Grades</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="student in students_grades">
                          <th scope="row">{{student.grade}}</th>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>
              <div class="col-lg-3 col-xl-3">
                <div class="card mb-5 mb-xl-0">
                  <div class="card-body p-5">
                    <table class="table">
                      <thead>
                        <tr>
                          <th scope="col">Students</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="student in courses_students">
                          <th scope="row">{{student.first_name}} {{student.last_name}}</th>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>
              <div class="col-lg-3 col-xl-3">
                <div class="card mb-5 mb-xl-0">
                  <div class="card-body p-5">
                    <table class="table">
                      <thead>
                        <tr>
                          <th scope="col">Teachers</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="teacher in courses_teachers">
                          <th scope="row">{{teacher.first_name}} {{teacher.last_name}}</th>
                        </tr>
                      </tbody>
                    </table>
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
                    <div class="col-lg-3 col-xl-3">
                      <div class="text-center mb-5">
                        <h2 class="fw-bolder">Remove Student</h2>
                      </div>
                      <form ame="removeStudent" @submit.prevent="removeStudent">
                        <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" v-model="removeStudentID">
                          <option value="" disabled selected>Select Student</option>
                          <option id="theStudent" v-for="student in students" :value="student.id">{{student.first_name}} {{student.last_name}}</option>
                        </select>
                        <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" v-on:click="removeStudent($event)" value="Submit">Remove</button></div>
                      </form>
                    </div>
<!-- Remove Teacher -->
                    <div class="col-lg-3 col-xl-3">
                        <div class="text-center mb-5">
                          <h2 class="fw-bolder">Remove Teacher</h2>
                        </div>
                        <form name="removeTeacher" @submit.prevent="removeTeacher">
                          <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" v-model="removeTeacherID">
                            <option value="" disabled selected>Select Teacher</option>
                            <option id="theTeacher" v-for="teacher in teachers" :value="teacher.id">{{teacher.first_name}} {{teacher.last_name}}</option>
                          </select>
                          <div class="d-grid"><button class="btn btn-primary btn-lg" id="submitButton" type="submit" v-on:click="removeTeacher($event)" value="Submit">Remove</button>
                        </div>
                      </form>
                    </div>
                    <div class="col-lg-3 col-xl-3">
                        <div class="text-center mb-5">
                          <h2 class="fw-bolder">Upload CSV</h2>
                        </div>
                        <form name="uploadFile" @submit.prevent="uploadFile">
                          <div class="form-group">
                            <input ref="file" v-on:change="handleFileUpload($event)"  type="file">
                          </div>
                          <br>
                          <button class="btn btn-primary btn-lg" v-on:click="submitFile($event)">Submit</button>
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
import FlatfileButton from "@flatfile/vuejs";
import { VueCsvImport } from "vue-csv-import";
import { ref} from "vue";

export default {
  name:'Add',
    setup() {
        const file = ref(null)
        const handleFileUpload = async() => {
           // debugger;
            console.log("selected file",file.value.files)
            //Upload to server
        }

        return {
          handleFileUpload,
          file
       }
    },

  data: function() {
    // licenseKey: 'a78e94bf-cdd5-417a-aa12-4f3f5c6aed0e';
    // settings: {
    //   type: 'test import';
    //   fields: [
    //     { label: 'Student ID', key: 'student_id' },
    //     { label: 'Course ID', key: 'course_id' },
    //     { label: 'Grade', key: 'grade' },
    //   ]
    // };

    // customer: {
    //   userId: '12345',
    // }

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

      students_grades: [],

      teacherID: "",
      studentID: "",
      studentcourseID: "",
      teachercourseID: "",

      teacherRemove: "",
      studentRemove: "",

      currentCourse: {},
      currentStudent: {},
      currentTeacher: {},

      file: '',

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
            this.students_grades = [];
            this.currentCourse = response.data;
            if (response.data.students && response.data.students.length) {
              this.courses_students = response.data.students;
              this.students_grades = response.data.student_courses;
              console.log("this is the grades", this.students_grades)
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
            this.students_grades = [];
            this.students_courses = [];
            this.courses_students = [];
            this.courses_teachers = [];
            this.currentStudent = response.data;
            if (response.data.courses && response.data.courses.length) {
              this.students_courses = response.data.courses;
              this.students_grades = response.data.student_courses;
              console.log("this is the grades", this.students_grades)

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
            this.students_grades = [];
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
      const {studentID, studentcourseID, grade } = this;
      console.log('assigning the student...', studentID, studentcourseID, grade);
      const params = { course_id: studentcourseID, grade: grade }
      axios.post(`/api/students/${studentID}`, params).then(response => {
        console.log('success!')

      })
    },

    assignTeacher: function(event) {
      event.preventDefault();
      const {teacherID, teachercourseID } = this;
      console.log('assigning the teacher...', teacherID, teachercourseID);
      const params = { course_id: teachercourseID }
      axios.post(`/api/teachers/${teacherID}`, params).then(response => {
        console.log('success!')
      })
    },

    removeStudent: function(event) {
      event.preventDefault();
      const {removeStudentID} = this;
      console.log('removing student...', removeStudentID);
      axios.delete("/api/students/" + removeStudentID).then(response => {
        this.students = this.students.filter((student)=> { return student.id !== removeStudentID })
      })

    },

    removeTeacher: function(event) {
      event.preventDefault();
      const {removeTeacherID} = this;
      console.log('removing teacher...', removeTeacherID);
      axios.delete("/api/teachers/" + removeTeacherID).then(response => {
        this.teachers = this.teachers.filter((teacher)=> { return teacher.id !== removeTeacherID })
      })
    },

    handleFileUpload: function() {
      event.preventDefault();
      this.file = this.$refs.file.files[0];
      console.log("this is the file I am uploading", this.file)
    },

    submitFile: function () {
      let formData = new FormData();
      formData.append('file', this.file);
      axios.post( '/api/students/import', formData,
        {
          headers: {
              'Content-Type': 'multipart/form-data'
          }
        }
      ).then(function(){
        console.log('SUCCESS!!');
      })
      .catch(function(){
        console.log('FAILURE!!');
      });

    }
  }
}
</script>
