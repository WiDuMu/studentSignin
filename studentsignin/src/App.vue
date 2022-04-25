<template>
  <div>
    <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
    <student-table v-bind:students="students" v-on:student-arrived-or-left="studentArrivedOrLeft" v-on:delete-student="deleteStudent"></student-table>
    <student-messege v-bind:student="mostRecentStudent"></student-messege>
  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessege from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'

export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessege,
    StudentTable
  },
  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },
  methods: {
    newStudentAdded(student) {
      this.students.push(student)
      this.students.sort(function(s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
      })
    },
    studentArrivedOrLeft(student, present) {
      let updateStudent = this.students.find( function(s) {
        if (s.name == student.name && s.starID == student.starID) {
          return true;
        }
      })

      if (updateStudent) {
        updateStudent.present = present
        this.mostRecentStudent = student
      }
    },
    deleteStudent(student) {
      this.students = this.students.filter ( function(s) {
        if (s != student) {
          return true
        }
      })
      this.mostRecentStudent = {}
    },
  }
}
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
