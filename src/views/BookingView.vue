<template>
    <div class="booking">
      <Navigation></Navigation>
      <h1 class="container p-5 ">Booking</h1>
  
      <div class="col-md-8">
                            <div class="form-group">
                             
                             <!-- <h1>{{id}}</h1> -->
                              
                                <div v-for="(pack,index) in packs">
                                  <hr>
                                      <p>Hotel - {{pack.user_name}}</p>
                                      <p>Room type - {{pack.room}}</p>
                                      <p>Room Inclusion - {{pack.inclusion}}</p>
                                      <p>Room Price (per day)- Rs.{{pack.price}}</p>
                                      <div class="input">
                                        <label for="std">Start Day
                                        <input 
                                          class="input-field"
                                          type="date" 
                                          name="std"
                                          v-model="std"/>
                                          </label>
                                        
                                        </div>
                                        <div class="input">
                                          <label for="quantity">Quantity:
                                        <input type="number" id="quantity" name="quantity" min="1" max="15"  v-model="num">
                                      </label>
                                        </div>
                                        <p>Total - Rs.{{pack.price*num}}</p>
                                      
                                       <button type="button" class="btn btn-success" @click="book(pack.user_name,pack.room,pack.price,std,num)">Book</button>
                                    
                                      <hr>
                                </div>
  
                            </div>
                          </div>
      
    </div>
  </template>
  <script>
    import firebase from 'firebase';
  import { db } from '../main';
  
  
  import Navigation from '@/components/NavigationComp.vue'
  import Footer from '@/components/FooterComp.vue'
  
  export default {
  name: 'BookingView',

     
  components: {

    Footer,
    Navigation
      },
      data(){
      return {
            id: 0,
            std:null,
            num:null,
            pack:null,
            packs:[]
  
       
              
      }
    },      
     created() {
            this.id = this.$route.params.id;
        },
    mounted: function() {
          
          this.fetchPacks();
          console.info('mounted, packs:', this.packs) 
      },
      computed: {
          items: function() {
              return this.packs
          }
      },
      methods:{
        fetchPacks () {
  
            db.collection('hotel_packages').get()
                .then(snapshot => {
                    snapshot.forEach( doc => {
                        const pack = doc.data()
                        // console.log(pack)
                        var id = doc.id;
                       
                             
                        this.pack=pack.room;
                      //   user.id = doc.id
                        this.packs.push(pack)
                       
                       
                    })
                })
                .catch(error => {
                    console.error(error)
                })
            console.debug('fetchUser return: ', this.users) 
        },
        book(userid,pack,price,std,num){
          
          var user = firebase.auth().currentUser;
          if (user != null){
            
           
            db.collection('bookings_hotelname').doc(user.uid).update({
              bookings:firebase.firestore.FieldValue.arrayUnion(userid),
         
            });
            db.collection('bookings_startdate').doc(user.uid).update({
              bookings:firebase.firestore.FieldValue.arrayUnion(std),
         
            });
            db.collection('bookings_roomtype').doc(user.uid).update({
              bookings:firebase.firestore.FieldValue.arrayUnion(pack),
         
            });
            db.collection('bookings_end').doc(user.uid).update({
              bookings:firebase.firestore.FieldValue.arrayUnion(num),
         
            });
            db.collection('bookings_price').doc(user.uid).update({
              bookings:firebase.firestore.FieldValue.arrayUnion(price*num),
         
            });
        
      }else{
  
                if (confirm("Please login")) {
                  this.$router.replace('/login');
                } 
                console.log("Document successfully written!");
      
      }
  
  
        },
        check(hotelid){
          console.log(hotelid);
        }
      }
  }
  </script>