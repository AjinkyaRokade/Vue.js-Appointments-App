<template>
  
    <div class="col-12 col-md-10 col-lg-7">
        <div class="input-group my-3">
          <input
            id="SearchApts"
            placeholder="Search"
            type="text"
            class="form-control"
            aria-label="Search Appointments"
            v-model="searchTerm"
          >
    
          <div class="input-group-append">
            <button
              type="button"
              class="btn btn-primary dropdown-toggle"
              data-bs-toggle='dropdown'
              aria-haspopup="true"
              aria-expanded="false"
            >
              Sort by
              <span class="caret"></span>
            </button>
    
            <div class="dropdown-menu dropdown-menu-right">
              <a @click="$emit('sendKey', 'petName')" class="dropdown-item d-flex justify-content-between" id="petName">
                Pet Name
                <font-awesome-icon icon="check" v-if="myKey==='petName'"/>
              </a>
    
              <a @click="$emit('sendKey', 'aptDate')" class="dropdown-item d-flex justify-content-between" href="#" id="aptDate">
                Date
                <font-awesome-icon icon="check" v-if="myKey==='aptDate'" />
              </a>
    
              <a @click="$emit('sendKey', 'petOwner')" href="#" class="dropdown-item d-flex justify-content-between" id="ownerName">
                Owner
                <font-awesome-icon icon="check" v-if="myKey==='petOwner'"/>
              </a>
    
              <div class="dropdown-divider" role="separator"></div>
    
              <a
                class="dropdown-item d-flex justify-content-between"
                href="#"
                id="asc" 
                @click="$emit('sendDir', 'asc')"
              >
                Asc
                <font-awesome-icon icon="check"  v-if="myDir==='asc'"/>
              </a>
    
              <a
                class="dropdown-item d-flex justify-content-between"
                href="#"
                id="desc"
                @click="$emit('sendDir', 'desc')"
              >
                Desc
                <font-awesome-icon icon="check" v-if="myDir==='desc'"/>
              </a>
            </div>
          </div>
        </div>
      </div>
      
</template>

<script>
  import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome"

export default{
    name:"SearchAppointments",
    data(){
        return{
            searchTerm:""
        }
    },
    props:["myKey", "myDir"],
    watch:{
        searchTerm:function(term){
            this.$emit("searchData", term);
        }
    },
    components:{
        FontAwesomeIcon
    },
    methods:{
      sendKey(key){
        this.$emit("sortKey", key)
      }
    }

}
</script>