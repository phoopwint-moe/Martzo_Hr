<template>
  <div>
    <div id="wrapper" :class="{ toggled: isClose }">
      <div id="page-content-wrapper">
        <div
          class="header-menu position-fixed w-100 top-0 z-2 bg-white py-1 shadow-sm"
        >
          <div class="row justify-content-center">
            <div class="col-md-10 d-flex justify-content-between p-2">
              <button
                @click="toggle()"
                type="button"
                class="hamburger animated fadeInLeft"
                :class="!isClose ? 'is-closed' : 'is-open'"
                data-toggle="offcanvas"
              >
                <span class="hamb-top"></span>
                <span class="hamb-middle"></span>
                <span class="hamb-bottom"></span>
              </button>
              <h5>{{ title }}</h5>
              <h5>Martzo HR</h5>
            </div>
          </div>
        </div>
        <div class="overlay" v-if="!isClose"></div>
        <nav
          class="navbar navbar-inverse fixed-top"
          id="sidebar-wrapper"
          role="navigation"
        >
          <ul class="nav sidebar-nav">
            <div class="sidebar-header">
              <div class="sidebar-brand d-flex justify-space-around">
                <div class="text-white">MartzoHR</div>
                <div @click="toggle()">
                  <i class="fa-solid fa-xmark text-white fs-2"></i>
                </div>
              </div>
            </div>
            <li
              @click="employee()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 2"
            >
              <i class="fa-solid fa-users me-3"></i>Employees
            </li>
            <li
              @click="department()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 2"
            >
              <i class="fa-solid fa-users me-3"></i>Department Mangement
            </li>
            <li
              @click="role()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 2"
            >
              <i class="fa-solid fa-briefcase me-3"></i>Role Mangement
            </li>
            <li
              @click="salary()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 2"
            >
              <i class="fa-solid fa-money-check-dollar me-3"></i>Salary
              Mangement
            </li>
            <li
              @click="attendance()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 2"
            >
              <i class="fa-solid fa-clock me-3"></i> Attendance Mangement
            </li>
            <li
              @click="attendanceOverview()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 2"
            >
              <i class="fa-solid fa-calendar-days me-3"></i>Attendance Overview
            </li>
            <li
              @click="project()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 3"
            >
              <i class="fa-solid fa-diagram-project me-3"></i>Project Mangement
            </li>
            <li
              @click="payroll()"
              v-if="userInfo.role_id == 1 || userInfo.role_id == 2"
            >
              <i class="fa-solid fa-calendar-check me-3"></i>Payroll
            </li>
          </ul>
        </nav>
      </div>
    </div>
  </div>
</template>
<script>
import { mapGetters } from "vuex";
export default {
  name: "AppHeader",
  props: ['title'],
  data() {
    return {
      isClose: false,
    };
  },
  computed: {
    ...mapGetters(["userInfo", "getToken"]),
  },
  methods: {
    employee() {
      this.$router.push({
        name: "employee",
      });
    },
    department() {
      this.$router.push({
        name: "department",
      });
    },
    role() {
      this.$router.push({
        name: "role",
      });
    },
    salary() {
      this.$router.push({
        name: "salary",
      });
    },
    attendance() {
      this.$router.push({
        name: "attendance",
      });
    },
    attendanceOverview() {
      this.$router.push({
        name: "attendanceOverview",
      });
    },
    payroll() {
      this.$router.push({
        name: "payroll",
      });
    },
    project() {
      this.$router.push({
        name: "project",
      });
    },
    toggle() {
      if (this.isClose == false) {
        this.isClose = true;
      } else {
        this.isClose = false;
      }
    },
  },
};
</script>

<style>
body {
  position: relative;
  overflow-x: hidden;
  background-color: #cfd8dc;
}

body,
html {
  height: 100%;
}
.nav .open > a,
.nav .open > a:hover,
.nav .open > a:focus {
  background-color: transparent;
}

/*-------------------------------*/
/*           Wrappers            */
/*-------------------------------*/

#wrapper {
  padding-left: 0;
  -webkit-transition: all 0.5s ease;
  -moz-transition: all 0.5s ease;
  -o-transition: all 0.5s ease;
  transition: all 0.5s ease;
}

#sidebar-wrapper {
  z-index: 1000;
  left: 250px;
  width: 0;
  height: 100%;
  margin-left: -250px;
  overflow-y: auto;
  overflow-x: hidden;
  background: #1a1a1a;
  -webkit-transition: all 0.5s ease;
  -moz-transition: all 0.5s ease;
  -o-transition: all 0.5s ease;
  transition: all 0.5s ease;
}

#sidebar-wrapper::-webkit-scrollbar {
  display: none;
}

#wrapper.toggled #sidebar-wrapper {
  width: 250px;
}

#page-content-wrapper {
  width: 100%;
}

#wrapper.toggled #page-content-wrapper {
  position: relative;
  margin-right: -250px;
}

/*-------------------------------*/
/*     Sidebar nav styles        */
/*-------------------------------*/
.navbar {
  padding: 0;
}

.sidebar-nav {
  position: absolute;
  top: 0;
  width: 250px;
  margin: 0;
  padding: 0;
  list-style: none;
}

.sidebar-nav li {
  position: relative;
  line-height: 20px;
  display: inline-block;
  width: 100%;
}

.sidebar-nav li:before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
  height: 100%;
  width: 3px;
  background-color: #db5656;
  -webkit-transition: width 0.2s ease-in;
  -moz-transition: width 0.2s ease-in;
  -ms-transition: width 0.2s ease-in;
  transition: width 0.2s ease-in;
}
.sidebar-nav li:hover {
  background: rgba(0, 0, 0, 0) !important;
  border-radius: 10px;
  margin-left: 10px;
  margin-right: 10px;
}
.sidebar-nav li:hover:before,
.sidebar-nav li.open:hover:before {
  width: 100%;
  -webkit-transition: width 0.2s ease-in;
  -moz-transition: width 0.2s ease-in;
  -ms-transition: width 0.2s ease-in;
  transition: width 0.2s ease-in;
}

.sidebar-nav li {
  display: block;
  color: #ddd;
  text-decoration: none;
  padding: 10px 15px 10px 30px;
}

.sidebar-nav li :hover,
.sidebar-nav li :active,
.sidebar-nav li :focus,
.sidebar-nav li.open :hover,
.sidebar-nav li.open :active,
.sidebar-nav li.open :focus {
  color: #fff;
  text-decoration: none;
  background-color: transparent;
}
.sidebar-header {
  text-align: center;
  font-size: 20px;
  position: relative;
  width: 100%;
  display: inline-block;
}
.sidebar-brand {
  height: 65px;
  position: relative;
  background: #191b22;
  background: linear-gradient(to right bottom, #2f3441 50%, #212531 50%);
  padding-top: 1em;
}
.sidebar-brand a {
  color: #ddd;
}
.sidebar-brand a:hover {
  color: #fff;
  text-decoration: none;
}
.dropdown-header {
  text-align: center;
  font-size: 1em;
  color: #ddd;
  background: #212531;
  background: linear-gradient(to right bottom, #2f3441 50%, #212531 50%);
}
.sidebar-nav .dropdown-menu {
  position: relative;
  width: 100%;
  padding: 0;
  margin: 0;
  border-radius: 0;
  border: none;
  background-color: #222;
  box-shadow: none;
}
.dropdown-menu.show {
  top: 0;
}
/*Fontawesome icons*/

/*-------------------------------*/
/*       Hamburger-Cross         */
/*-------------------------------*/

.hamburger {
  position: relative;
  z-index: 999;
  display: block;
  width: 32px;
  height: 32px;
  margin-left: 15px;
  background: transparent;
  border: none;
}
.hamburger:hover,
.hamburger:focus,
.hamburger:active {
  outline: none;
}
.hamburger.is-closed:before {
  content: "";
  display: block;
  width: 100px;
  font-size: 14px;
  color: #fff;
  line-height: 32px;
  text-align: center;
  opacity: 0;
  -webkit-transform: translate3d(0, 0, 0);
  -webkit-transition: all 0.35s ease-in-out;
}
.hamburger.is-closed:hover:before {
  opacity: 1;
  display: block;
  -webkit-transform: translate3d(-100px, 0, 0);
  -webkit-transition: all 0.35s ease-in-out;
}

.hamburger.is-closed .hamb-top,
.hamburger.is-closed .hamb-middle,
.hamburger.is-closed .hamb-bottom,
.hamburger.is-open .hamb-top,
.hamburger.is-open .hamb-middle,
.hamburger.is-open .hamb-bottom {
  position: absolute;
  left: 0;
  height: 4px;
  width: 100%;
}
.hamburger.is-closed .hamb-top,
.hamburger.is-closed .hamb-middle,
.hamburger.is-closed .hamb-bottom {
  background-color: #1a1a1a;
}
.hamburger.is-closed .hamb-top {
  top: 5px;
  -webkit-transition: all 0.35s ease-in-out;
}
.hamburger.is-closed .hamb-middle {
  top: 50%;
  margin-top: -2px;
}
.hamburger.is-closed .hamb-bottom {
  bottom: 5px;
  -webkit-transition: all 0.35s ease-in-out;
}

.hamburger.is-closed:hover .hamb-top {
  top: 0;
  -webkit-transition: all 0.35s ease-in-out;
}
.hamburger.is-closed:hover .hamb-bottom {
  bottom: 0;
  -webkit-transition: all 0.35s ease-in-out;
}
.hamburger.is-open .hamb-top,
.hamburger.is-open .hamb-middle,
.hamburger.is-open .hamb-bottom {
  background-color: #1a1a1a;
}
.hamburger.is-open .hamb-top,
.hamburger.is-open .hamb-bottom {
  top: 50%;
  margin-top: -2px;
}
.hamburger.is-open .hamb-top {
  -webkit-transform: rotate(45deg);
  -webkit-transition: -webkit-transform 0.2s cubic-bezier(0.73, 1, 0.28, 0.08);
}
.hamburger.is-open .hamb-middle {
  display: none;
}
.hamburger.is-open .hamb-bottom {
  -webkit-transform: rotate(-45deg);
  -webkit-transition: -webkit-transform 0.2s cubic-bezier(0.73, 1, 0.28, 0.08);
}
.hamburger.is-open:before {
  content: "";
  display: block;
  width: 100px;
  font-size: 14px;
  color: #fff;
  line-height: 32px;
  text-align: center;
  opacity: 0;
  -webkit-transform: translate3d(0, 0, 0);
  -webkit-transition: all 0.35s ease-in-out;
}
.hamburger.is-open:hover:before {
  opacity: 1;
  display: block;
  -webkit-transform: translate3d(-100px, 0, 0);
  -webkit-transition: all 0.35s ease-in-out;
}

/*-------------------------------*/
/*            Overlay            */
/*-------------------------------*/
</style>
