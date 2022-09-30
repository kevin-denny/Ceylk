<template>
    <div class="hotelinfo">
      <Navigation></Navigation>
      <h1>{{hname}}</h1>
  
      <div class="col-md-8">
                            <div class="form-group">
                              <p>{{description}}</p>
                              <ul>
                                <li>Address        - {{address}}</li>
                                <li>Contact Person - {{repname}}</li>
                                <li>Contact No     - {{phn}}</li>
                              </ul>
                             
                            <div class="form-group d-flex justify-content-start">
                              <div class="d-flex justify-content-start p-2" v-for="image in images">
                                <img :src="image" alt=""  style="width: 300px;">
                              </div>
                            </div>
                            <div style="background: #dce0dd;" class="container p-5 my-5 border rounded">
                            <h4>Enter Review</h4>
                            <br>
                              <div>
                                <div class="form-group">
                                  <textarea v-model="addreviews" placeholder="Enter Review" class="form-control"></textarea>
                                  
                                </div>
                              </div>
                            <br>
                            <button type="button" class="btn btn-light" @click="addReview(id)" >Add Review</button>
                              
                            </div>
                            <div class="form-group d-flex">
                              <div class="container my-5 border rounded" >
                                <h3>Reviews</h3>
                                <div v-for="(review,index) in reviews">
                                  
                                    <div style="background: #ebedeb;" class="container p-5 border">
                                      <p >{{review}}</p>
                                      
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
            hname:null,
            address:null,
            description:null,
            phn:null,
            addreviews:[],
            reviews:[],
            repname:null,
            images:[],
          
  
       
              
      }
    },      
    created() {
            this.id = this.$route.params.id;
         
            this.check(this.id);
         

        },
    mounted: function() {
        
        
      
    },

  
      methods:{
    

        check(id){
        db.collection('hotel_bios').doc(id).get().then(doc=>{
            this.hname=doc.data().hotel_name,
            this.description=doc.data().descript,
            this.address=doc.data().address,
            this.phn=doc.data().phn_no,
            this.images=doc.data().hotelpics
            

        });

        db.collection('hotel_profiles').doc(id).get().then(doc=>{
            this.repname=doc.data().rep_name,
            this.reviews=doc.data().reviews
          

        });
      
     
          
        },
        addReview(id){
          db.collection('hotel_profiles').doc(id).update({
            reviews:firebase.firestore.FieldValue.arrayUnion(this.addreviews)


          }).then(function(){
            console.log("Success");
          }).then(function() {
              if (confirm("Successfully added")) {
                  window.location.reload();
              } 
              console.log("Document successfully written!");
              })
        },
 
      }
  }
  </script>