<template>
    <div class="col-12">
        <div class="card textcenter mt-3">
          <div class="card-header bg-primary text-white">
            <button class="btn btn-primary"  @click="visibility=!visibility"><font-awesome-icon icon="plus" class="mr-3" />Add Appointment</button>
          </div>
    <div v-if="visibility">
          <div class="card-body">
            <!-- <form id="aptForm" @submit.prevent="$emit('addAppt', formData)"> -->
                <form id="aptForm" @submit.prevent="requestAdd">
              <div class="form-group form-row">
                <label class="col-md-2 col-form-label text-md-right" for="petName">Pet Name</label>
                <div class="col-md-10" style="margin: auto;">
                  <input
                    type="text"
                    class="form-control"
                    name="petName"
                    id="petName"
                    placeholder="Pet's Name"
                    v-model="formData.petName" 
                  >
                </div>
              </div>
    
              <div class="form-group form-row">
                <label class="col-md-2 col-form-label text-md-right" for="ownerName">Pet Owner</label>
                <div class="col-md-10" style="margin: auto;">
                  <input type="text" class="form-control" id="ownerName" placeholder="Owner's Name"  v-model="formData.petOwner">
                </div>
              </div>
    
              <div class="form-group form-row "  >
                <label class="col-md-2 col-form-label text-md-right" for="aptDate"> Date</label>
                <div class="col-md-4" style="margin: auto;" >
                  <input type="date" class="form-control" id="aptDate"  v-model="formData.aptDate">
                </div>
                <label class="col-md-2 col-form-label text-md-right" for="aptTime">Time</label>
                <div class="col-md-4" style="margin: auto;" >
                  <input type="time" class="form-control" name="aptTime" id="aptTime"  v-model="formData.aptTime">
                </div>
              </div>
    
              <div class="form-group form-row">
                <label class="col-md-2 text-md-right" for="aptNotes">Apt. Notes</label>
                <div class="col-md-10" style="margin: auto;">
                  <textarea
                    class="form-control" 
                    rows="4"
                    cols="50"
                    name="aptNotes"
                    id="aptNotes"
                    placeholder="Appointment Notes"
                    v-model="formData.aptNotes"
                  ></textarea>
                </div>
              </div>
    
              <div class="form-group form-row mb-0" >
                <div class="offset-md-2 col-md-10" >
                  <button style="margin: auto;" type="submit" class="btn btn-primary d-block ml-auto">Add Appointment</button>
                </div>
              </div>
            </form>
          </div>
        </div>
        </div>
      </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome"

export default{
    name:"AddAppointment",
    data(){
        return{
            // formData:{
            //     petName:"",
            //     petOwner:"",
            //     aptDate:"",
            //     aptNotes:"",
            //     aptTime:""
            // },
            formData:{},
            visibility:false
        }
    },
    components:{
        FontAwesomeIcon
    },
    methods:{
        requestAdd(){
            this.formData={
                petName:this.formData.petName,
                petOwner:this.formData.petOwner,
                aptDate:this.formData.aptDate+" "+this.formData.aptTime,
                aptNotes:this.formData.aptNotes,
            }
            this.$emit("addAppt", this.formData)
            this.formData={}
            this.visibility=false;
        }
    }
}
</script>