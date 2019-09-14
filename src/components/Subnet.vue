<template>
  <div class="hello">
    <h1>Subnet</h1>
    <div>
      <form>
          <label for="subnetmask1">Enter IP:</label>
        <div class="flex-container">
          <div class="">
            <input v-model="maskOctet1" class="form-control" id="subnetmask1"  placeholder="255" :class="{ 'alert-danger':maskOctet1_error}">
          </div>
          <span>.</span>
          <div class="">
            <input v-model="maskOctet2" class="form-control" id="subnetmask2"  placeholder="255" :class="{ 'alert-danger':maskOctet2_error}">
          </div>
          <span>.</span>
          <div class="">
            <input v-model="maskOctet3" class="form-control" id="subnetmask3"  placeholder="255" :class="{ 'alert-danger':maskOctet3_error}">
          </div>
          <span>.</span>
          <div class="">
            <input v-model="maskOctet4" class="form-control" id="subnetmask4"  placeholder="255" :class="{ 'alert-danger':maskOctet4_error}">
          </div>
          <span> / </span>
          <div class="">
            <input v-model="network_number" class="form-control" id="network_number"  placeholder="24" :class="{ 'alert-danger':network_number_error}">
          </div>
        </div>

        <div  class="flex-errors">
          <div v-if="show_octetErrormsg" class="error threeThirds">
           <small><i> {{ error_msg }}</i></small>
          </div>
          <div v-if="show_networkNumberErrormsg" class="error third float-right ml-2 ">
            <small><i>{{ network_number_error_msg }}</i></small>
          </div>
        </div>
        <!--<div type="submit" class="cal-btn mt-4" @click="claculateHosts">Calculate</div>-->
        <div type="submit" class="btn btn-primary mt-4" @click="claculateHosts">Calculate</div>
      </form>



      <div class="mt-3">
        <div class="alert alert-info">
          <div>{{ binaryMask }}</div>
          <div>{{ hosts }}</div>
          <div> {{ netClass }}</div>
        </div>
      </div>
      <div class="mt-3">
        <div class="alert alert-warning">
          <div>{{ test }}</div>
          <div>{{ binary_ip }}</div>
        </div>
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
        binary_ip: [],
        network_number: '',
        network_number_error: false,
        network_number_error_msg: 'number between 0-30',
        maskOctet1: '',
        maskOctet1_error: false,
        maskOctet2: '',
        maskOctet2_error: false,
        maskOctet3: '',
        maskOctet3_error: false,
        maskOctet4: '',
        maskOctet4_error: false,
        netClass: 'NetClass',
        hosts: 'number of hosts',
        binaryMask: 'binary mask',
        test: 'Testing Area',
        error_msg: 'number between 0-255',
    }
  },
  methods: {
      claculateHosts() {
//            this.hosts = this.maskOctet1 +'.'+ this.maskOctet2 +'.'+ this.maskOctet3 +'.'+ this.maskOctet4
//            this.hosts = this.maskOctet1.toString(2)
          this.validateInput();
          if(this.maskOctet1_error || this.maskOctet2_error || this.maskOctet3_error || this.maskOctet4_error || this.network_number_error){
              ;
          }
          this.hosts = (this.maskOctet1 >>> 0).toString(2)
              + '.' + (this.maskOctet2 >>> 0).toString(2)
              + '.' + (this.maskOctet3 >>> 0).toString(2)
              + '.' + (this.maskOctet4 >>> 0).toString(2)

          this.binary_ip.push((this.maskOctet1 >>> 0).toString(2))
          this.binary_ip.push((this.maskOctet2 >>> 0).toString(2))
          this.binary_ip.push((this.maskOctet3 >>> 0).toString(2))
          this.binary_ip.push((this.maskOctet4 >>> 0).toString(2))

          this.binaryMask = (this.maskOctet1 >>> 0).toString(2)
              + '.' + (this.maskOctet2 >>> 0).toString(2)
              + '.' + (this.maskOctet3 >>> 0).toString(2)
              + '.' + (this.maskOctet4 >>> 0).toString(2)

          this.netClass = (this.maskOctet1 >>> 0).toString(2).substring(0, 3)

          this.test = (this.maskOctet1 >>> 0).toString(2)
              + (this.maskOctet2 >>> 0).toString(2)
              + (this.maskOctet3 >>> 0).toString(2)
              + (this.maskOctet4 >>> 0).toString(2)

//          this.test += ' length: '+this.test.length
//          this.test += 'UUU: ' + (parseInt(this.network_number) - 1)
          this.test = this.test.substring(parseInt(this.network_number)-1)
      },
      validateInput() {
//          if (this.maskOctet1 === "") {
//              this.maskOctet1_error = true;
//              this.test = 'is empty: ' + this.maskOctet1;
//          }

          if (isNaN(this.maskOctet1) || this.maskOctet1 < 0 || this.maskOctet1 > 255 || this.maskOctet1.length === 0) {
              this.maskOctet1_error = true;
          } else {
              this.maskOctet1_error = false;
          }
          if (isNaN(this.maskOctet2) || this.maskOctet2 < 0 || this.maskOctet2 > 255 || this.maskOctet2.length === 0) {
              this.maskOctet2_error = true;
          } else {
              this.maskOctet2_error = false;
          }
          if (isNaN(this.maskOctet3) || this.maskOctet3 < 0 || this.maskOctet3 > 255 || this.maskOctet3.length === 0) {
              this.maskOctet3_error = true;
          } else {
              this.maskOctet3_error = false;
          }
          if (isNaN(this.maskOctet4) || this.maskOctet4 < 0 || this.maskOctet4 > 255 || this.maskOctet4.length === 0) {
              this.maskOctet4_error = true;
          } else {
              this.maskOctet4_error = false;
          }
          if (isNaN(this.network_number) || this.network_number < 0 || this.network_number > 255 || this.network_number.length === 0) {
              this.network_number_error = true;
          } else {
              this.network_number_error = false;
          }
      },

      resetValues(){

      }
  },

  computed: {

      show_octetErrormsg(){
          if (this.maskOctet1_error || this.maskOctet2_error || this.maskOctet3_error || this.maskOctet4_error ){
              return true;
          }
          return false;
      },
      show_networkNumberErrormsg(){
          if (this.network_number_error){
              return true;
          }
          return false;
      }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .cal-btn{
    padding: 5px;
    width: 100px;
    margin: auto;
    background-color: cadetblue ;
    border: 1px solid cadetblue;
    border-radius: 5px;
  }
  .flex-container{
    display: flex;
    justify-content: center;
    margin-bottom: -7px;
  }
  .flex-container > div{
    flex: 6;
    /*min-width: 30px;*/
  }

  .flex-container > span{
    font-size: 2rem;
  }
  .flex-errors {
    display: flex;
    width: 100%;
  }
  /*.flex-errors > div{*/
    /*flex: 2;*/
  /*}*/
  .threeThirds{
    width: 80%;
  }
  .third{
    width: 20%;
  }

.error{
  padding: 5px;
  color: crimson;
  border: 1px solid crimson;
  border-radius: 4px;
  text-align: center;

}


</style>
