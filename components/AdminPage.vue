<script>
import firebase from 'firebase/app'
import '@firebase/firestore'
import '@firebase/auth'
var ref;
let temp = [];
const fireDb = firebase.firestore()
export default {
  data(){
		return{
			Arruser: []
		}
	},
  	created(){
		ref = fireDb.collection('users')
    ref.get().then(function(querySnapshot) {
      temp = querySnapshot.docs
    }).catch(function(error) {
                console.log("Error getting document:", error);
    });
	},
    mounted(){
    setTimeout(() => {
       temp.forEach(dosa => {
        let appData = dosa.data();
        //let nice = [];
        appData.id = dosa.id;
        this.Arruser.push(appData);
        console.log(this.Arruser.phone)
        //alert(appData.phone)
        //nice.push(appData.phone);
        // nice.forEach(element => {
        //   for (let index = 0; index < element.length; index++) {
        //       Arruser.push(appData);
        //   }
        // });
      });
     }, 700);
  },
  	methods: {
		deleteUser(doc){
			if(confirm('Are you shure?')){
        fireDb.collection("users").doc(doc).delete().then(() => {
          console.log("Document successfully deleted!");
        }).catch((error) => {
          console.error("Error removing document: ", error);
        });
      }else{

      }
		}
	}
}
</script>


<template>
<div class="products">
      <div class="container">
          <div class="product-test">
            <!-- <button @click="addNew" class="btn btn-primary float-right">Add User</button> -->
            <div class="table-responsive">
                <table class="table">
                  <thead>
                    <tr>
                      <th>Name</th>
                      <th>Lastname</th>
                      <th>Phone</th>
                      <th>Modify</th>
                    </tr>
                  </thead>
                  <tbody>
                      <tr v-for="users in Arruser" v-bind:key="users">
                        <td>
                          {{users.firstname}}
                        </td>
                        <td>
                          {{users.lastname}}
                        </td>
                        <td>
                          {{users.phone}}
                        </td>
                        <td id="Edit">
                          <!-- <button  class="btn btn-primary" @click="editUser()">Edit</button> -->
                          <button class="btn btn-danger" @click="deleteUser(users.id)">Delete</button>
                        </td>
                      </tr>
                  </tbody>
                </table>
            </div>
          </div>
      </div>
</div>
</template>