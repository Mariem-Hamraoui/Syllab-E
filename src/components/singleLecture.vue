<template>
  <div class =' singlelecture'>
    
    <h7>   <p> Lecture Name : </p> <strong> {{ req.name }} </strong> </h7>

    <br />
    <br />
     <br />
    <button id="ShowDt"  @click= "toggleDetails"> Show Details </button>
     <br />
      <br />
    <ul v-if= "detailsAreVisible">
      <div>
        <br />

        <strong id ='lbldash' > Description: </strong>
        <br />
        {{ req.description }}
      </div>
      <br />
      <div>
        <br />
        <button id="ShowDt"   @click="deleteLecture(req.name)"> Delete </button>
        <br />
        <br />
        <div id ='lbldash'> List of staff already invited </div>
        <br />
        <input
          type="email"
          name="staffemail"
          id="staffemail"
          v-model= "staffemail"
          placeholder="  Enter your Staff email .. "
        />
        <br />
         <br />
        <button  id="ShowDt"   type="submit" @click= "inviteStaff"> Add Staff </button>
        <br />
      </div>
    </ul>
    <br />
    <br />
  </div>
</template>

<script>
import firebase from "firebase";

export default {
  props: ["req"],
  data() {
    return {
      detailsAreVisible: false,
      staffemail: "",
    };
  },

  methods: {
    toggleDetails() {
      this.detailsAreVisible = !this.detailsAreVisible;
    },
    async inviteStaff() {
      ///create the staff user if it doesn't existe else location the userID

      const url =
        "https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=AIzaSyBi_DKlPK32LxHFHunBlrknvthTiTGyTrw";

      const response = await fetch(url, {
        method: "POST",
        body: JSON.stringify({
          email: this.staffemail,
          password: "p@ssw0rd",
          returnSecureToken: true,
        }),
      });

      const responseData = await response.json();
      const userId = responseData.localId;
      console.log(userId);
      if (!response.ok) {
        const error = new Error(
          responseData.message || "Failed to create - user already added"
        );
        throw error;
      }

      ///add the lecture the new staff user account
      const newLecture = {
        quiz: this.req.quiz,
        name: this.req.name,
        fileUrl: this.req.fileUrl,
        description: this.req.description,
        userRole: "staff",
      };
      const staffresponse = await fetch(
        `https://syllab-e-default-rtdb.europe-west1.firebasedatabase.app/lectures/${userId}.json`,
        {
          method: "POST",
          body: JSON.stringify(newLecture),
        }
      );

      const staffresponseData = await response.json();

      if (!response.ok) {
        const error = new Error(
          responseData.message || "Failed to send Lecture."
        );
        throw error;
      }
    },

    deleteLecture(doc) {
      console.log(firebase);
    },
  },
};
</script>
<style>
.lbldash{
  font-size: 14px;
}
.singlelecture{
  padding-left : 5px ;
   box-sizing: none ;
   margin : 0 auto ;  
}

h7 {
font-size: 20px ;
} 

#ShowDt {
  width: 40%;
}

</style>