<template>
    <div class="console">
       <div class="container">
           <div class="intro h-100">
              <div class="row h-100 align-items-center">
                <div class="col-md-6 ml-3">
                      <h3>Reports</h3>
                      <h4>Hotels</h4>
                      
                      <br>
                     
  
                          <div class="col-md-8">
                            <div class="form-group">
                                <table ref="export_table">
              <thead>
                <tr>
                  <td>Hotel Name</td>
                  <td>Rep Name</td>
                  <td>Payment</td>
                </tr>
              </thead>
              <tbody  >
                <tr v-for="(name,index) in users">
                  <td>{{name.hotel_name}}</td>
                  <td>{{name.rep_name}}</td>
                  <td>{{name.paid}}</td>

                </tr>
              </tbody>
            </table>
            <br>
            <br>
            <button class="btn btn-success"  @click="ExportExcel('xlsx')">Export</button>

            


  
                            </div>
                          </div>
                          <br>
                          <h4>Tourists</h4>
                      
                      <br>
                     
  
                          <div class="col-md-8">
                            <div class="form-group">
                                <table ref="export_Ttable">
              <thead>
                <tr>
                  <td>Tourist Name</td>
                  <td>Country</td>
                  <td>DOB</td>
                </tr>
              </thead>
              <tbody  >
                <tr v-for="(tname,index) in tusers">
                  <td>{{tname.user_name}}</td>
                  <td>{{tname.country}}</td>
                  <td>{{tname.dob}}</td>

                </tr>
              </tbody>
            </table>
            <br>
            <br>
            <button class="btn btn-success"  @click="ExportTExcel('xlsx')">Export</button>

            


  
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
      name: "AdminReport",
    
    
      data(){
        return {
          name:null,
          tname:null,
            users: [],
            tusers:[]
    
         
                
        }
      }
      ,
        mounted: function() {
          const recaptchaScript = document.createElement("script");
        recaptchaScript.setAttribute(
          "src",
          "https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.11/min.js"
        );
        document.head.appendChild(recaptchaScript);
        const recaptchaScrip = document.createElement("script");
        recaptchaScript.setAttribute(
          "src",
          "https://unpkg.com/xlsx@0.15.1/dist/xslx.full.min.js"
        );
        document.head.appendChild(recaptchaScrip);
    
    
            
        this.fetchUsers()
            console.info('mounted, users:', this.users) 
        this.fetchTUsers()
            console.info('mounted, users:', this.tusers) 
        },
        computed: {
            items: function() {
                return this.packs
            }
        },
      methods:{
        async fetchUsers () {
              await db.collection('hotel_profiles').get()
                  .then(snapshot => {
                      snapshot.forEach( doc => {
                          const user = doc.data()
                          if(user.verify=="no"){
                         
                               
                               this.name=user.hotel_name;
                          user.id = doc.id
                          this.users.push(user)
                          }
                         
                      })
                  })
                  .catch(error => {
                      console.error(error)
                  })

              console.debug('fetchUser return: ', this.users) 
          },
          async fetchTUsers () {
              await db.collection('tourist_profiles').get()
                  .then(snapshot => {
                      snapshot.forEach( doc => {
                          const tuser = doc.data()
                        
                         
                               
                        
                          tuser.id = doc.id
                          this.tusers.push(tuser)
                        
                         
                      })
                  })
                  .catch(error => {
                      console.error(error)
                  })

              console.debug('fetchUser return: ', this.tusers) 
          },
          ExportExcel(type,fn,dl){
            var XLSX = require("xlsx");
            var elt =this.$refs.export_table;
            var wb = XLSX.utils.table_to_book(elt, {sheet:"Sheet JS"});
            return dl ?
              XLSX.write(wb, {bookType:type, bookSST:true, type: 'base64'}):
            XLSX.writeFile(wb, fn || ('HotelUser.' + (type || 'xlsx')));
          },
          ExportTExcel(type,fn,dl){
            var XLSX = require("xlsx");
            var elt =this.$refs.export_Ttable;
            var wb = XLSX.utils.table_to_book(elt, {sheet:"Sheet JS"});
            return dl ?
              XLSX.write(wb, {bookType:type, bookSST:true, type: 'base64'}):
            XLSX.writeFile(wb, fn || ('TouristUser.' + (type || 'xlsx')));
          }
      }
    }
    </script>
    <style>
    td,tbody,tr,thead{
      border:1px solid black 
    }
    
    </style>