
<style>
@media screen and (max-width: 800px) {
  .is-responsive {
    width: 100%;
    border-collapse: collapse;
    border-spacing: 0;
    display: block;
    position: relative;
  }
  .is-responsive td:empty:before {
    content: " ";
  }
  .is-responsive th,
  .is-responsive td {
    margin: 0;
    vertical-align: top;
  }
  .is-responsive th {
    text-align: left;
  }
  .is-responsive thead {
    border-right: solid 2px #dbdbdb;
    display: block;
    float: left;
  }
  .is-responsive thead tr {
    display: block;
    padding: 0 10px 0 0;
  }
  .is-responsive thead tr th::before {
    content: " ";
  }
  .is-responsive thead td,
  .is-responsive thead th {
    border-width: 0 0 1px;
  }
  .is-responsive tbody {
    display: block;
    width: auto;
    position: relative;
    overflow-x: auto;
    white-space: nowrap;
  }
  .is-responsive tbody tr {
    display: inline-block;
    vertical-align: top;
  }
  .is-responsive th {
    display: block;
    text-align: right;
  }
  .is-responsive td {
    display: block;
    min-height: 1.25em;
    text-align: left;
  }
  .is-responsive th:last-child,
  .is-responsive td:last-child {
    border-bottom-width: 0;
  }
  .is-responsive tr:last-child td:not(:last-child) {
    border: 1px solid #dbdbdb;
    border-width: 0 0 1px;
  }
  .is-responsive.is-bordered td,
  .is-responsive.is-bordered th {
    border-width: 1px;
  }
  .is-responsive.is-bordered tr td:last-child,
  .is-responsive.is-bordered tr th:last-child {
    border-bottom-width: 1px;
  }
  .is-responsive.is-bordered tr:last-child td,
  .is-responsive.is-bordered tr:last-child th {
    border-width: 1px;
  }
}
</style>


<template lang="pug">
  #app.section
    form-json(:formFields="jsonFields",
              :formName="'userProfile'")
    table(class='table is-responsive' v-if='resultantObj.length != 0')
      thead
        tr
          th UID
          th Source
          th Destination
          th Direction
          th Sent Time
          th Total Cost
          th Status
      tbody
        tr(v-for="item in resultantObj")
          td {{item.uid}}
          td {{item.source}}
          td {{item.destination}}
          td {{item.direction}}
          td {{item.sentTime}}
          td {{item.totalCost}}
          td {{item.status}}
</template>

<script>
import "bulma/css/bulma.min.css";
import "vue-form-json/dist/vue-form-json.css";
import Bar from "vue-chartjs";
import formJson from "vue-form-json";
import jsonFields from "@/assets/fields";
import axios from "axios";

export default {
  extends: Bar,
  name: "app",
  components: {
    formJson
  },
  data: () => ({
    jsonFields,
    resultantObj: ""
  }),

  mounted() {
    this.$root.$on("formSubmitted", values => {
      axios({
        method: "get",
        url: "https://api.karix.co/message/",
        auth: {
          username: "6e9d22a6-2e74-439e-888b-721fa1312251",
          password: "e8e7f546-b60d-4efd-9717-9766f4f087b9"
        }
      }).then(response => {
        // console.log(response.data.objects);
        var responseObj = response.data.objects;
        var res = JSON.stringify(responseObj);
        const result = JSON.parse(res).map(item => {
          return {
            uid: item.account_uid,
            source: item.source,
            destination: item.destination,
            direction: item.direction,
            sentTime: item.sent_time,
            totalCost: item.total_cost,
            status: item.status
          };
        });
        this.resultantObj = result;
      });
    });

    //http get request using axios
  }
};
</script>
