<template >
  <div id="main-app" class="container" style="BlinkMacSystemFont">
    <h1 >Appointments</h1>
    <div class="row justify-content-center">
      <AddAppointment style="margin-bottom: 10mm" @addAppt="addAppt" />
      <SearchAppointments
        @searchData="searchAppointments"
        :myKey="filterKey"
        :myDir="filterDir"
        @sendKey="setKey"
        @sendDir="setDir"
      />
      <h4>{{ title }}</h4>
      <AppointmentList
        :appointments="filteredAppointment"
        @deleteAppointment="deleteAppt"
        @edit="editData"
      ></AppointmentList>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import AddAppointment from "./components/AddAppointment";
import AppointmentList from "./components/AppointmentList";
import SearchAppointments from "./components/SearchAppointments.vue";
import _ from "lodash";
import Swal from "sweetalert2";

export default {
  name: "MainApp",
  data() {
    return {
      title: "Appointment List",
      appointments: [],
      aptIndex: 1,
      searchTerms: "",
      filterKey: "petName",
      filterDir: "asc",
    };
  },
  components: {
    AppointmentList,
    AddAppointment,
    SearchAppointments,
  },
  mounted() {
    // console.log("mounted")
    axios.get("http://localhost:3000/Appointments").then(
      (response) =>
        (this.appointments = response.data.map((item) => {
          item.aptId = this.aptIndex;
          this.aptIndex++;
          return item;
        }))
    );
  },
  computed: {
    searchedAppts() {
      if (this.searchTerms != "")
        return this.appointments.filter((item) => {
          return (
            item.petName.toLowerCase().match(this.searchTerms.toLowerCase()) ||
            item.petOwner.toLowerCase().match(this.searchTerms.toLowerCase()) ||
            item.aptNotes.toLowerCase().match(this.searchTerms.toLowerCase())
          );
        });
      else return this.appointments;
    },
    filteredAppointment() {
      return _.orderBy(
        this.searchedAppts,
        (item) => item[this.filterKey].toLowerCase(),
        this.filterDir
      );
    },
  },
  methods: {
    deleteAppt(item) {
      Swal.fire({
        title: "Do you want to delete?",
        showDenyButton: true,
        confirmButtonText: "Delete",
        denyButtonText: `Cancel`,
      }).then((result) => {
        if (result.isConfirmed) {
          this.appointments = _.without(this.appointments, item);
          axios.delete(`http://localhost:3000/Appointments/${item.id}`);
          Swal.fire("Deleted uccessfully!", "", "success");
        } else if (result.isDenied) {
          Swal.fire("Cancelled", "", "info");
        }
      });
      // this.appointments.splice(index, 1)
    },
    editData(id, field, text) {
      const apt = { [field]: text };
      console.log(id);
      axios.patch(`http://localhost:3000/Appointments/${id}`, apt);
      const index = this.appointments.findIndex((item) => item.aptId == id);
      console.log(index);
      this.appointments[index][field] = text;
    },
    addAppt(formData) {
      Swal.fire({
        title: "Do you want to Add?",
        showDenyButton: true,
        confirmButtonText: "Add",
        denyButtonText: `Cancel`,
      }).then((result) => {
        if (result.isConfirmed) {
          //  formData.aptId=this.aptIndex;
      //  this.aptIndex++;
      console.log(formData);
      axios.post("http://localhost:3000/Appointments", formData).then(
        // this.appointments.push(formData)
      );
          Swal.fire("Added Successfully!", "", "success");
        } else if (result.isDenied) {
          Swal.fire("Cancelled", "", "info");
        }
      });
      
    },
    searchAppointments(terms) {
      this.searchTerms = terms;
    },
    setKey(key) {
      this.filterKey = key;
    },
    setDir(dir) {
      this.filterDir = dir;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
