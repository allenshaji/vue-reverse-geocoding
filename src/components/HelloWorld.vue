<template>
  <div class="col-md-12">
    <div class="card card-box card-danger">
      <div class="card-header">
        <h2>Enter latitude and Longitude</h2>
      </div>
      <form>
        <div class="card-body">
          <div class="row">
            <div class="col-md-3">
              <div class="form-group">
                <label>Enter Latitude</label>
                <input id="latVillage" type="text" class="form-control" placeholder="Lattitude of Village" name="latVillage" required v-model="lat" pattern="^[0-9]+\.[0-9][0-9][0-9][0-9]$" maxlength="7" oninvalid="this.setCustomValidity('Enter a valid Latitude')" oninput="this.setCustomValidity('')">
              </div>
            </div>
            <div class="col-md-3">
              <div class="form-group">
                <label>Enter Longitude</label>
                <input id="lngVillage" type="text" class="form-control" placeholder="Longitude of Village" name="lngVillage" required="" v-model="lng" pattern="^[0-9]+\.[0-9][0-9][0-9][0-9]$" maxlength="7" oninvalid="this.setCustomValidity('Enter a valid Longitude')" oninput="this.setCustomValidity('')">
              </div>
            </div>

          </div>
          <div class="row">
            <div class="col-md-4">
              <div class="form-group">
                <label>Address</label>
                <input id="district" type="text" class="form-control" placeholder="Address" name="address" required="" v-model="address">
              </div>
            </div>
            <div class="col-md-4">
              <div class="form-group">
                <label>State</label>
                <input id="state" type="text" class="form-control" placeholder="State" name="state" required="" v-model="state">
              </div>
            </div>
            <div class="col-md-4">
              <div class="form-group">
                <label>Country</label>
                <input id="country" type="text" class="form-control" placeholder="Country" name="country" required="" v-model="country">
              </div>
            </div>
          </div>
          <div v-if="isDiabled"></div>
        </div>

      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'add',
  data() {
    return {
      postalcode:'',
      country:'',
      state: '',
      address: '',
      lat: '',
      lng: '',
      errors: []
    }
  },
          computed: {
             isDiabled() {
          if (this.lat.length > 0 && this.lng.length > 0) {
           axios.get('https://maps.googleapis.com/maps/api/geocode/json?latlng=' + this.lat + ',' + this.lng)
        .then(response => {
            if(response.data.results.length  == 0){
                this.address = '';
                this.state = '';
                this.country = '';
            }
            else{
                this.address = response.data.results[0].formatted_address;
                for(var i =0;i<=response.data.results[0].address_components.length;i++)
            {
            
            if(response.data.results[0].address_components[i].types[0] == 'administrative_area_level_1')
            {
               this.state = response.data.results[0].address_components[i].long_name;
            }
            else if(response.data.results[0].address_components[i].types[0] == 'country'){
              this.country = response.data.results[0].address_components[i].long_name;

            }
            }
            }
        
        //console.log(this.state);
        })
        .catch(e => {
        this.errors.push(e)
      })
    } else {
         return true;
        }
            }
        },
}
</script>
