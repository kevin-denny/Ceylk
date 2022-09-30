<template>
    <div class="HotelPackages">
        <div class="container">
            
          <div class="intro h-100">
              <div class="row h-100 align-items-center">
                <div class="col-md-6 ml-3">
                      <h3>Packages</h3>
         
                   
              

                  
  
  
                </div>
              
             </div>
             <div >  





                <div class="container">
                    <div class="row">
                    
                    
                    <div class="col-md-8">
                        <div class="form-group">
                        <input type="text"  v-model="room" placeholder="Room Type" class="form-control">
                        </div>
                    </div>
                <br>
                <br>
                    <div class="col-md-8">
                        <div class="form-group">
                        <input v-model="price" placeholder="Price" class="form-control">
                        <!-- <input type="text"  v-model="profile.address" placeholder="Address" class="form-control"> -->
                        </div>
                    </div>
                <br>
                <br>
                    <div class="col-md-8">
                        <div class="form-group">
                        <input type="text"  v-model="inclu" placeholder="Inclusion(Full Board, Half Board, B & B)" class="form-control">
                        </div>
                    </div>
                <br>
                <br>
                    

                    <div class="col-md-8">
                        <div class="form-group">
                            <input type="submit" @click="addpack" value="Add Package" class="btn btn-primary w-100">
                        </div>
                    </div>
                <br>
                <br>
                    </div>
                    <div class="col-md-8">
                          <div class="form-group">
                            
                              <!-- <button type="button" class="btn btn-success w-100"><a id="link" href="https://console.firebase.google.com/project/testvue-ad06c/overview">Visit Console</a></button> -->
                              <div v-for="(pack,index) in packs">
                                <hr>
                                    <p>Hotel Name - {{hotelname}}</p>
                                    <p>Room type - {{pack.room}}</p>
                                    <p>Room Price - {{pack.price}}</p>
                                    <p>Room Inclusion - {{pack.inclusion}}</p>
                                    
                                    <div class="d-flex justify-content-around">
                                     
                                    
                                  
                                    </div>
                                    <hr>
                              </div>

                          </div>
                        </div>
                </div>
                </div>
  
        </div>
    </div>
  </div>
      

  </template>

<script>
import firebase from 'firebase';
import { db } from '../main';

export default {
  name: "HotelPackages",

  data(){
    return {
       
          room:null,
          hotelname:null,
          price:null,
          inclu:null,
          pack:null,
          packs:[]

     
            
    }
  },created(){
    this.check()
  }
  ,
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
    check(){
      var user = firebase.auth().currentUser;
       
       db.collection('hotel_profiles').doc(user.uid).get().then(doc => {
     this.hotelname=doc.data().hotel_name
   
   })
    },
   fetchPacks () {
        var userid = firebase.auth().currentUser;

          db.collection('hotel_packages').get()
              .then(snapshot => {
                  snapshot.forEach( doc => {
                      const pack = doc.data()
                     
                      const id = doc.id;

                           
                      this.pack=pack.room;
                  
                      this.packs.push(pack)
      
                     
                  })
              })
              .catch(error => {
                  console.error(error)
              })
          console.debug('fetchUser return: ', this.users) 
      },
    addpack(){
        var user = firebase.auth().currentUser;
       



        db.collection('hotel_packages').doc().set({
          
          user_name: this.hotelname,
          user_id: user.uid,
          room: this.room,
          price: this.price,
          inclusion: this.inclu
        })
        .then(function() {
         
         console.log("Document successfully written!");
         
         if (confirm("Successfully Added")) {
                                  window.location.reload();
                              } 
       })
       .catch(function(error) {
         console.error("Error writing document: ", error);
       });
    },

  }
}
</script>