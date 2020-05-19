<template>
  <div class="container has-text-centered dashbody pb-2">
    <div class="container">
      <section class="hero is-primary mt-2">
        <div class="hero-body">
          <div class="container">
            <h1 class="title">
              Collector Dashboard
            </h1>
            <h2 class="subtitle">
              Welcome Buuruwa
            </h2>
          </div>
        </div>
      </section>

      <section class="mt-2">
        <h1 class="title is-4">Current Requests</h1>
        <div class=" container req-table has-text-centered">
          <table class="table is-fullwidth">
            <thead>
              <tr>
                <th>Request No.</th>
                <th>Item Category</th>
                <th>Sub Category</th>
                <th>Address</th>
                <th>Province</th>
                <th>Time</th>
                <th>Date</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody>
                <tr
                  v-for="(job, index) in jobslist"
                  v-bind:key= "index"
                >
                  <td>{{ job.ref.toString().substr(0,7) }}</td>
                  <td>{{ job.itemCategory }}</td>
                  <td>{{ job.subCategory }}</td>
                  <td>{{ job.pickupAddress }}</td>
                  <td>{{ job.customerProvince }}</td>
                  <td>{{ job.time }}</td>
                  <td>{{ job.date }}</td>
                  <td>
                    <button class="button is-small is-warning hvr-pulse-grow">Reserve</button>
                  </td>
                </tr>
              <tr v-if="this.jobslist.length=='0'">
                <td colspan="7" style="text-align: center">You have no active jobs</td>
              </tr>
              
            </tbody>
          </table>
        </div>
      </section>
    </div>
  </div>
</template>


<script>
/* eslint-disable */
import { firebaseApp,firebase } from '../firebase'

export default {
    name: 'Dashboard',
    data(){
      return{
        jobslist: [],
        notify: false,
        notifyStatus: ''
      }
    },
    created(){

      //let loggedUserEmail = firebaseApp.auth().currentUser.email
      let thisState = this

      firebaseApp.firestore().collection("jobs").where(
        'status',
        "==",
        'pending'
      )
      .onSnapshot(callback =>{
          thisState.jobslist = []
          callback.forEach(function(doc){
          //Append document ID into job object
            let tempData = doc.data()
            tempData = Object.assign(tempData,{ref: doc.id.toString()})
            thisState.jobslist.push(tempData)
        })
      },
      error =>{
        console.log(error)
      }
      )
    }
}
</script>

<style scoped>
  @import url('../assets/css/style.css');

  /* Pulse Grow */
  @-webkit-keyframes hvr-pulse-grow {
    to {
      -webkit-transform: scale(1.1);
      transform: scale(1.1);
    }
  }
  @keyframes hvr-pulse-grow {
    to {
      -webkit-transform: scale(1.1);
      transform: scale(1.1);
    }
  }
  .hvr-pulse-grow {
    display: inline-block;
    vertical-align: middle;
    -webkit-transform: perspective(1px) translateZ(0);
    transform: perspective(1px) translateZ(0);
    box-shadow: 0 0 1px rgba(0, 0, 0, 0);
  }
  .hvr-pulse-grow:hover, .hvr-pulse-grow:focus, .hvr-pulse-grow:active {
    -webkit-animation-name: hvr-pulse-grow;
    animation-name: hvr-pulse-grow;
    -webkit-animation-duration: 0.3s;
    animation-duration: 0.3s;
    -webkit-animation-timing-function: linear;
    animation-timing-function: linear;
    -webkit-animation-iteration-count: infinite;
    animation-iteration-count: infinite;
    -webkit-animation-direction: alternate;
    animation-direction: alternate;
  }
</style>