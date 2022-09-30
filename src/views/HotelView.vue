<template>
  <div class="hotel">
    <Navigation></Navigation>
    <h1 class="container p-5 ">Hotels</h1>

    <div>
                          <div class="form-group">
                            

                              <div v-for="(hotel,index) in hotels">
                               
                                <hr>
                                <div class="container p-5 my-5 border">
                                  <div  class=" d-flex justify-content-around ">  
                                    <div class="container pr-6">
                                      <img class="rounded" :src="hotel.profilepic" style="width: 400px;"  alt="User picture">
                                    </div>
                                    
                                  <div>
                                    <p class="h4"><b>{{hotel.hotel_name}}</b></p>
                                    <p>Description - {{hotel.descript}}</p>
                                    <div class="form-group d-flex justify-content-start">
                                    <div class="d-flex justify-content-start p-2" v-for="image in hotel.hotelpics">
                                      <img :src="image" alt=""  style="width: 100px;">
                                    </div>
                                  </div>
                                    <button type="button" class="btn btn-light" ><router-link :to="{ name: 'hotelinfo', params: { id: hotel.id } }">View more >></router-link></button>
                                  </div>
                                  </div>


                                    
                                  
                                    <hr>
                              </div>
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
name: 'HotelView',


   
components: {
  
    Footer,
    Navigation,
   
},
    data(){
    return {
          std:null,
          num:null,
          hotel:null,
          hotels:[],
          images:[]

     
            
    }
  },
  mounted: function() {
        
        this.fetchHotels();
        console.info('mounted, packs:', this.packs) 
    },
    computed: {
        items: function() {
            return this.packs
        }
    },
    methods:{
      fetchHotels () {

          db.collection('hotel_bios').get()
              .then(snapshot => {
                  snapshot.forEach( doc => {
                      const hotel = doc.data()
                      
                      var id = doc.id;
                     
                           
                      //  this.hotel=hotel.room;
                       console.log(hotel.profilepic)
                       hotel.id = doc.id
                      this.hotels.push(hotel)
                     
                     
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
          db.collection('bookings').doc(user.uid).set({
            user_id:user.uid,
            hotel:userid,
            start:std,
            end:num,
            price:price,
          })
      
    }else{

              if (confirm("Please login")) {
                this.$router.replace('/login');
              } 
              console.log("Document successfully written!");
    
    }


      },
      viewmore(hotelid){
        console.log(hotelid);
        
      }
    }
}
</script>