<template>
  <div class="hello">
    <div>
      <form>
          <label for="subnetmask">Enter Subnetmask:</label>
        <div class="form-group form-row">
          <div class="col-sm-3 mb-2">
            <input v-model="maskinput" type="email" class="form-control" id="subnetmask" aria-describedby="emailHelp" placeholder="255">
          </div>
          <div class="col-sm-3 mb-2">
            <input v-model="maskinput" type="email" class="form-control" id="subnetmask" aria-describedby="emailHelp" placeholder="255">
          </div>
          <div class="col-sm-3 mb-2">
            <input v-model="maskinput" type="email" class="form-control" id="subnetmask" aria-describedby="emailHelp" placeholder="255">
          </div>
          <div class="col-sm-3 mb-2">
            <input v-model="maskinput" type="email" class="form-control" id="subnetmask" aria-describedby="emailHelp" placeholder="255">
          </div>
          <!--<small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>-->
        </div>

        <span type="submit" class="btn btn-primary" @click="claculateHosts">Calculate</span>
      </form>
      <div class="mt-3">
        <div class="alert alert-info">{{ hostsToMask }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Subnet',
  props: {
    msg: String
  },
  data(){
    return{
        maskinput: '',
        hostsToMask: 'number of hosts',
    }
  },
  methods: {
      claculateHosts(){

            this.hostsToMask = isNaN(this.maskinput)
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cal-btn{
  padding: 5px;
  border: 1px solid blue;
  border-radius: 3px;
}


</style>
