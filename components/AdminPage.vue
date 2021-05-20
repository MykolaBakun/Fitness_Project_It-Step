<script>
import firebase from 'firebase/app'
import '@firebase/firestore'
import '@firebase/auth'
var ref;
var refEvent;
let temp = [];
let temp2 = [];
const fireDb = firebase.firestore()
export default {
  
  data(){
		return{
			Arruser: [],
      Arrevents: [],
      data: "",
      description: "",
      imgSrc: "",
      title: ""
		}
	},
  	created(){
		ref = fireDb.collection('users')
    ref.get().then(function(querySnapshot) {
      temp = querySnapshot.docs
    }).catch(function(error) {
                console.log("Error getting document:", error);
    });
    refEvent = fireDb.collection('events')
    refEvent.get().then(function(querySnapshot) {
      temp2 = querySnapshot.docs
    }).catch(function(error) {
                console.log("Error getting document:", error);
    });
	},
    mounted(){
    setTimeout(() => {
       temp.forEach(dosa => {
        let appData = dosa.data();
        appData.id = dosa.id;
        this.Arruser.push(appData);
        console.log(this.Arruser.phone)
      });
      temp2.forEach(dosa => {
        let appData = dosa.data();
        appData.id = dosa.id;
        this.Arrevents.push(appData);
        console.log(this.Arrevents)
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
		},
      setup() {
    fireDb.collection("events").doc().set({
    color: "black",
    data: this.data,
    description: this.description,
    event: true,
    imgSrc: this.imgSrc,
    title: this.title
    
})
.then(() => {
    console.log("Document successfully written!");
})
.catch((error) => {
    console.error("Error writing document: ", error);
});
  }
	}
}
</script>


<template>
<div class="userpanel">
      <div class="container">
        <b-field label="Title" message="">
						<b-input type="text"  v-model="title"  />
				</b-field>
        <b-field label="Description" message="">
						<b-input type="text"  v-model="description" />
				</b-field>
        <b-field label="Data" message="">
						<b-input type="text"  v-model="data" />
				</b-field>
        <b-field label="Img" message="">
					  <b-input type="text"  v-model="imgSrc" />
				</b-field>
        <b-button type="is-link" @click="setup()">Create</b-button>
          <div class="product-test">

            <!-- <button @click="addNew" class="btn btn-primary float-right">Add User</button> -->
            <div class="table-responsive">
                <table class="table">
                  <thead>
                    <tr>
                      <th>Title</th>
                      <th>Date</th>
                      <th>Showen</th>
                      <!-- <th>Modify</th> -->
                    </tr>
                  </thead>
                  <tbody>
                      <tr v-for="event in Arrevents" v-bind:key="event">
                        <td>
                          {{event.title}}
                        </td>
                        <td>
                          {{event.date}}
                        </td>
                        <td>
                          {{event.event}}
                        </td>
                        <!-- <td id="Edit">
                          <button  class="btn btn-primary" @click="editUser()">Edit</button>
                          <button class="btn btn-danger" @click="deleteUser(users.id)">Delete</button>
                        </td> -->
                      </tr>
                  </tbody>
                </table>
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