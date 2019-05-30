
<style>
#table-wrapper {
  display: table;
}
#desc {
  /* width: 15em; */
  border-collapse: collapse;
  border-radius: 50px;
}
#desc th {
  background-color: whitesmoke;
  padding: 1em;
  text-align: center;
  border: 1px solid black;
  padding: 5px;
  width: 100%;
}

#desc td {
  border: 1px solid black;
  padding: 5px;
  text-align: center;
}
</style>


<template lang="pug">
  #app.section
    form-json(:formFields="jsonFields",
              :formName="'userProfile'")

    #table-wrapper
      table#desc(v-if="resultantObj.length!==0")
        tr
          th(colspan=7) User Information
        tr
          td UID
          td Source
          td Destination
          td Direction
          td Sent Time
          td Total Cost
          td Status
        tr(v-for="item in resultantObj" v-model="resultantObj")
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
import formJson from "vue-form-json";
import jsonFields from "@/assets/fields";
import axios from "axios";

export default {
  name: "app",
  components: {
    formJson
  },
  data: () => ({
    jsonFields,
    resultantObj: [{}]
  }),

  mounted() {
    this.$root.$on("formSubmitted", values =>
      // console.log(JSON.stringify(values)
      {
        // axios
        //   .get("https://api.karix.co/message/")
        //   .then(response => console.log(response));
        axios({
          method: "get",
          url: "https://api.karix.co/message/",
          auth: {
            username: "6e9d22a6-2e74-439e-888b-721fa1312251",
            password: "e8e7f546-b60d-4efd-9717-9766f4f087b9"
          }
        }).then(function(response) {
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
          var temp = JSON.stringify(result);
          console.log(temp);
          this.resultantObj = temp;
        });
      }
    );
    //http get request using axios
  }
};
</script>
