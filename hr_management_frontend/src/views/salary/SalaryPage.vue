<template>
  <div>
    <Header title="Salary"></Header>
    <div class="col-md-8 my-15 mx-auto p-3">
      <button class="btn btn-primary" @click="goCreate()">
        <i class="fa-regular fa-square-plus"></i>
        Add Salary
      </button>
      <div class="card mt-3">
        <div class="card-body p-4">
          <v-row>
            <v-col md="5">
              <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                density="compact"
                label="Search"
                single-line
                flat
                hide-details
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col>
              <v-data-table
                :headers="headers"
                :items="items"
                :items-length="totalItems"
                :search="search"
                :items-per-page="itemsPerPage"
                class="p-3 elevation-1"
                :loading="loading"
                @update:options="getData"
              >
                <!-- action buttons  -->
                <template v-slot:[`item.actions`]="{ item }">
                  <v-icon small @click="updatePage(item.id)" class="me-1"
                    >mdi-pencil</v-icon
                  >
                  <v-icon
                    small
                    @click="deleteData(item.id)"
                    v-if="userInfo.role_id == 1"
                    class="me-1 text-danger"
                    >mdi-delete</v-icon
                  >
                </template>
              </v-data-table>
            </v-col>
          </v-row>
        </div>
      </div>
    </div>

    <Footer></Footer>
  </div>
</template>

<script>
import axios from "axios";
import { mapGetters } from "vuex";
import Header from "../AppHeader.vue";
import Footer from "../FooterPage.vue";
import Swal from "sweetalert2";
import moment from "moment";
export default {
  name: "SalaryPage",
  components: {
    Header,
    Footer,
  },
  data() {
    return {
      message: "",
      search: "",
      headers: [
        { title: "Employee", value: "name" },
        { title: "Year", value: "year" },
        { title: "Month", value: "month" },
        { title: "Amount", value: "amount" },
        { title: "Actions", value: "actions" },
      ],
      items: [],
      loading: "",
      itemsPerPage: 5,
      total: 0,
    };
  },
  computed: {
    ...mapGetters(["userInfo", "getToken"]),
  },
  methods: {
    getData() {
      this.loading = true;
      axios
        .get("http://localhost:8000/api/salaryList")
        .then((response) => {
          let data = response.data;

          for (let i = 0; i < data.length; i++) {
            data[i].month = moment(data[i].month).format("MMM");
            data[i].amount = data[i].amount + " Ks";
          }
          console.log(data);
          this.items = data;
          this.total = data.length;
          this.loading = false;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    goCreate() {
      this.$router.push({
        name: "create#salary",
      });
    },
    messageAlert(mes) {
      if (mes !== undefined && mes !== "" && mes !== null) {
        Swal.fire({
          title: "success!",
          text: mes,
          icon: "success",
        });
      }
      this.$router.replace({ query: null });
    },
    updatePage(id) {
      this.$router.push({
        name: "update#salary",
        query: {
          id: id,
        },
      });
    },
    deleteData(id) {
      console.log(id);
      Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!",
      }).then((result) => {
        if (result.isConfirmed) {
          axios
            .get("http://localhost:8000/api/salary/delete/" + id)
            .then((response) => {
              console.log(response);
              Swal.fire({
                title: "Deleted!",
                text: response.data,
                icon: "success",
              });
              this.getData();
            })
            .catch((e) => {
              console.log(e);
            });
        }
      });
    },
  },

  mounted() {
    this.message = this.$route.query.message;
    this.getData();
    this.messageAlert(this.message);
  },
};
</script>
<style>
.v-pagination__list {
  margin-bottom: 0;
}
</style>
