<template>

        <vue-cal class="container"
         :time-step="30"
         :time-from="7 * 60"
         :time-to="23 * 60"
         :disable-views="['years', 'year', 'month']"
         hide-weekends
         editable-events
         resize-x
         :min-event-width="minEventWidth"
         :events="events"
         @event-change="onChange">
        </vue-cal>



</template>




<script>



import VueCal from 'vue-cal'
import 'vue-cal/dist/vuecal.css'
import db from '@/firebase/init'
import firebase from 'firebase'
import moment from "moment";

export default {
 


  components: {
      VueCal
  },

data: () => ({
        
        start: null,
        end: null,
        selectedEvent: {},
        showDialog: false,
        minEventWidth: 0,
  events: [
  
  ]


}),

    created(){
      this.getEvents();
    },
    
methods: {
    onChange(promijenjeniEvent) {
      console.log(promijenjeniEvent.id),
                  console.log(promijenjeniEvent.title);
                  //var user = firebase.auth().currentUser;
                  //if(user){
                  db.collection("termini")
                          .doc(this.$route.params.emailSalona)//////
                          .collection("Kalendar")
                          .doc(promijenjeniEvent.id)
                          .update({ 
                            title:promijenjeniEvent.title, });
                            console.log("Uspješno dodano ime");
                            alert('Uspješno ste rezervirali termin');
                            //}
    },
    getEvents() {
      //var user = firebase.auth().currentUser;
      //if (user) {
        db.collection("termini")
          .doc(this.$route.params.emailSalona)///
          .collection("Kalendar").get()
          //.doc('slobodni_termini')
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              const data = doc.data();
              console.log(data)
              let start = moment(data.start.toDate()).format("YYYY-MM-DD HH:mm")
              let end = moment(data.end.toDate()).format("YYYY-MM-DD HH:mm")
              
                const termin = {
                  start: start,
                  end: end,
                  id: doc.id,
                  title: data.title,
                  content: data.content
                };
                console.log(termin);
                this.events.push(termin);
             
            });
          });
      //}
    }

  }




    
}
</script>





<style >
    .vuecal{
      background-color: white;
    }

    .vuecal__event {
    color: white;
    background-color:#654321;
    position: relative;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    left: 0;
    width: 100%;
    z-index: 1;
    -webkit-transition: left .3s,width .3s,-webkit-box-shadow .3s;
    transition: left .3s,width .3s,-webkit-box-shadow .3s;
    transition: box-shadow .3s,left .3s,width .3s;
    transition: box-shadow .3s,left .3s,width .3s,-webkit-box-shadow .3s;
    overflow: hidden;
}



</style>