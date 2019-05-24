
<style>
#table-wrapper {
  display: table;
}
#desc {
  /* width: 15em; */
  border-collapse: collapse;
}
#desc th {
  background-color: whitesmoke;
  padding: 1em;
  text-align: center;
  border: 1px solid black;
  padding: 1em;
  width: 100%;
}

#desc td {
  border: 1px solid black;
  padding: 1em;
}
</style>


<template lang="pug">
  #app.section
    form-json(:formFields="jsonFields",
              :formName="'userProfile'")

    #table-wrapper
      table#desc
        tr
          th User Information
        tr
          td UID
          td Source
          td Destination
          td Direction
          td Sent Time
          td Total Cost
          td Status
        tr
          td(v-for="item in tempArr") {{item.src}}
       
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
    jsonFields
  }),

  mounted() {
    this.$root.$on("formSubmitted", values =>
      // console.log(JSON.stringify(values)
      {
        var tempArr = [];
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

          // alert(JSON.stringify(responseObj));
          var res = JSON.stringify(responseObj);
          // console.log(res);
          res = JSON.parse(res);
          for (let i in res) {
            console.log(res[i]);
            tempArr.push(res[i]);
          }
          console.log(tempArr);
          // const result = JSON.parse(res).map(item => {
          //   return {
          //     uid: item.uid,
          //     source: item.source,
          //     destination: item.destination,
          //     direction: item.direction,
          //     sentTime: item.sent_time,
          //     totalCost: item.total_cost,
          //     status: item.status
          //   };
          // });
          // console.log(result);
        });
      }
    );
    //http get request using axios
  }
};
</script>
