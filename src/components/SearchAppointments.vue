<template>
  <div class="col-12 col-md-10 col-lg-7">
    <div class="input-group my-3">
      <input
        id="SearchApts"
        placeholder="Search"
        type="text"
        class="form-control"
        aria-label="Search Appointments"
        v-model="searchTerms"
      />

      <div class="input-group-append">
        <button
          type="button"
          class="btn btn-primary dropdown-toggle"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false"
        >
          Sort by
          <span class="caret"></span>
        </button>

        <div class="dropdown-menu dropdown-menu-right">
          <a href="#" class="dropdown-item d-flex justify-content-between" id="petName"
          @click="$emit('requestKey','petName')"
          >
            Pet Name
            <font-awesome-icon icon="check"/>
          </a>

          <a class="dropdown-item d-flex justify-content-between" href="#" id="aptDate"
          @click="$emit('requestKey','aptDate')">
            Date
            <font-awesome-icon icon="check" v-if="MyKey==='aptDate'" />
          </a>

          <a href="#" class="dropdown-item d-flex justify-content-between" id="ownerName"
          @click="$emit('requestKey','petOwner')">
            Owner
            <font-awesome-icon icon="check" v-if="MyKey==='petOwner'"/>
          </a>

          <div class="dropdown-divider" role="separator"></div>

          <a class="dropdown-item d-flex justify-content-between" href="#" id="asc"
          @click="$emit('requestDir','asc')">
            Asc
            <font-awesome-icon icon="check" v-if="MyDir==='asc'"/>
          </a>

          <a class="dropdown-item d-flex justify-content-between" href="#" id="desc"
             @click="$emit('requestDir','desc')">
            Desc
            <font-awesome-icon icon="check" v-if="MyDir==='desc'"/>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

Vue.component("font-awesome-icon", FontAwesomeIcon);

export default {
  name: "searchAppointments",
  data: function() {
    return {
      searchTerms: ""
    };
  },
  watch: {
    searchTerms: function() {
      this.$emit("searchItem", this.searchTerms);
    }
  },
  props:["MyKey", "MyDir"]
};
</script>