<template>
  <div id="main-app" class="container">
    <div class="row justify-content-center">
      <add-appointment @add="addItem" />
      <search-appointments @searchItem="searchAppointments" :MyKey="filterKey" :MyDir="filterDir" @requestKey="changeKey" @requestDir="changeDir"/>
      <appointments-list :appointments="filterAppointments" @remove="removeItem" @edit="editItem" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import AppointmentsList from "./components/AppointmentList";
import AddAppointment from "./components/AddAppointment";
import SearchAppointments from "./components/SearchAppointments";
import _ from "lodash";
export default {
  name: "MainApp",
  data: function() {
    return {
      appointments: [],
      appIndex: 0,
      searchTerms: "",
      filterKey: "petName",
      filterDir: "asc"
    };
  },
  components: {
    AppointmentsList,
    AddAppointment,
    SearchAppointments
  },
  mounted() {
    axios.get("./data/appointments.json").then(
      response =>
        (this.appointments = response.data.map(item => {
          item.appId = this.appIndex;
          this.appIndex++;
          return item;
        }))
    );
  },
  computed: {
    searchApts: function() {
      return this.appointments.filter(item => {
        return (
          item.petName.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.petOwner.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.aptNotes.toLowerCase().match(this.searchTerms.toLowerCase())
        );
      });
    },
    filterAppointments: function() {
      return _.orderBy(
        this.searchApts,
        item => {
          return item[this.filterKey].toLowerCase();
        },
        this.filterDir
      );
    }
  },
  methods: {
    removeItem: function(apt) {
      this.appointments = _.without(this.appointments, apt);
    },
    editItem: function(id, field, text) {
      const aptIndex = _.findIndex(this.appointments, { appId: id });
      this.appointments[aptIndex][field] = text;
    },
    addItem: function(formData) {
      formData.appId = this.appIndex++;
      this.appointments.push(formData);
    },
    searchAppointments: function(searchItem) {
      this.searchTerms = searchItem;
    },
    changeKey: function(value){
      this.filterKey = value;
    },
    changeDir: function(dir){
      this.filterDir=dir;
    }

  }
};
</script>
