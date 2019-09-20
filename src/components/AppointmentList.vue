<template>
  <div class="col-12 col-md-10 col-lg-7">
    <div class="list-group list-group-flush">
      <div
        class="list-group-item d-flex align-items-start"
        v-for="item in appointments"
        v-bind:key="item.appId"
      >
        <button class="mr-2 btn btn-sm btn-danger" @click="$emit('remove', item)">
          <font-awesome-icon icon="trash" />
        </button>
        <div class="w-100">
          <div class="d-flex justify-content-between">
            <span class="h4 text-primary" contenteditable="contecteditable" @blur="$emit('edit',item.appId, 'petName', $event.target.innerText)">{{item.petName}}</span>
            <span class="float-right">{{formattedDate(item.aptDate)}}</span>
          </div>
          <div class="owner-name">
            <span class="font-weight-bold text-primary mr-1">Owner:</span>
            <span  contenteditable="contecteditable"
              @blur="$emit('edit',item.appId, 'petOwner', $event.target.innerText)">{{item.petOwner}}</span>
          </div>
          <div>{{item.aptNotes}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import moment from "moment";
Vue.component("font-awesome-icon", FontAwesomeIcon);
export default {
  name: "AppointmentsList",
  props: ["appointments"],

  methods: {
    formattedDate: function(date) {
      return moment(new Date(date)).format("MM-DD-YY h:mm a");
    }
  }
};
</script>