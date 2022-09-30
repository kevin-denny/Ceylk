<template>
    <div class="touristbooking">
        <div class="container">
            <h4>Bookings</h4>

<div class="col-md-8">
  <div class="form-group">
     
      <div v-for="(hname,index) in hnames">
        <hr>
            <p>Hotel - {{hname}}</p>
            <p>Start day - {{starts[index]}}</p>
            <p>Number of days - {{ends[index]}}</p>
            <p>Room type - {{roomtypes[index]}}</p>
            <p>Price - Rs.{{prices[index]}}</p> 

            <hr>
      </div>

  </div>
</div>

        </div>
    </div>
</template>
<script>
  import { db } from '../main';
  import firebase from 'firebase';

export default {
    
  name: "TouristBooking",
  data(){
      return {
        name:null,
        prices:[],
        hnames:[],
        ends:[],
        roomtypes:[],
        starts:[]
      }
    },
    created(){
        
        this.fetchhnames()
    },
    mounted: function() {



},

methods: {

        fetchhnames (){
        var user = firebase.auth().currentUser;
         db.collection('bookings_hotelname').doc(user.uid).get().then(doc=>{
            this.hnames=doc.data().bookings   
            
        });
        db.collection('bookings_price').doc(user.uid).get().then(doc=>{
            this.prices=doc.data().bookings   

        }).then(()=>{
            console.log(user.uid)
        });
        db.collection('bookings_end').doc(user.uid).get().then(doc=>{
            this.ends=doc.data().bookings   
            
        });
        db.collection('bookings_roomtype').doc(user.uid).get().then(doc=>{
            this.roomtypes=doc.data().bookings   
            
        });
        db.collection('bookings_startdate').doc(user.uid).get().then(doc=>{
            this.starts=doc.data().bookings   
            
        });
        
             
      }


    }

} 
</script>