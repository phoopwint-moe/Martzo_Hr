<template>
  <div>
    <Header title="My Project"></Header>
    <div class="col-md-10 my-15 mx-auto p-3">
      <div class="card mt-3">
        <div class="card-body p-4">
          <v-row>
            <v-col md="3">
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
                <!-- show img column  -->
                <template v-slot:[`item.leader`]="{ item }">
                  <v-card class="my-2" elevation="2" rounded>
                    <v-img :src="item.leader" cover></v-img>
                  </v-card>
                </template>

                <template v-slot:[`item.members`]="{ item }">
                  <v-avatar
                    v-for="(member, index) in item.members"
                    :key="index"
                    :image="member"
                    size="small"
                  >
                  </v-avatar>
                </template>
                <!-- priority and status column  -->
                <template v-slot:[`item.priority`]="{ item }">
                  <div class="text-center">
                    <v-chip
                      :color="
                        item.priority == 'low'
                          ? 'green'
                          : item.priority == 'medium'
                          ? 'orange'
                          : 'red'
                      "
                      :text="item.priority"
                      class="text-uppercase"
                      label
                      size="small"
                    ></v-chip>
                  </div>
                </template>

                <template v-slot:[`item.status`]="{ item }">
                  <div class="text-center">
                    <v-chip
                      :color="
                        item.status == 'pending'
                          ? 'orange'
                          : item.status == 'in_progress'
                          ? 'blue'
                          : 'green'
                      "
                      :text="item.status"
                      class="text-uppercase"
                      label
                      size="small"
                    ></v-chip>
                  </div>
                </template>

                <!-- action buttons  -->
                <template v-slot:[`item.id`]="{ item }">
                  <v-icon
                    small
                    @click="detailPage(item.id)"
                    class="me-1 text-primary"
                    >mdi-information-outline</v-icon
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
export default {
  name: "MyProjectPage",
  components: {
    Header,
    Footer,
  },
  data() {
    return {
      message: "",
      search: "",
      headers: [
        { title: "Title", value: "title" },
        { title: "Description", value: "description" },
        { title: "Leader", value: "leader" },
        { title: "Members", value: "members" },
        { title: "Start Date", value: "start_date" },
        { title: "Deadline", value: "deadline" },
        { title: "Priority", value: "priority" },
        { title: "Status", value: "status" },
        { title: "Actions", value: "id" },
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
        .get("http://localhost:8000/api/myprojectList/" + this.userInfo.id)
        .then((response) => {
          let data = response.data;
          this.items = data.projects;
          for (let i = 0; i < this.items.length; i++) {
            this.items[i].members = [];
            for (let j = 0; j < data.projectMembers[i].length; j++) {
              if (data.projectMembers[i][j].img == null) {
                data.projectMembers[i][j].img =
                  "http://localhost:8000/image/default.jpg";
              } else {
                data.projectMembers[i][j].img =
                  "http://localhost:8000/image/" +
                  data.projectMembers[i][j].img;
              }

              if (data.projectMembers[i][j].role == "leader") {
                this.items[i].leader = "";
                this.items[i].leader = data.projectMembers[i][j].img;
              }
              if (data.projectMembers[i][j].role == "member") {
                this.items[i].members.push(data.projectMembers[i][j].img);
              }
            }
          }

          this.total = this.items.length;
          this.loading = false;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    detailPage(id) {
      this.$router.push({
        name: "detail#project",
        query: {
          id: id,
        },
      });
    },
  },

  mounted() {
    this.getData();
  },
};
</script>
<style>
.v-pagination__list {
  margin-bottom: 0;
}
</style>
