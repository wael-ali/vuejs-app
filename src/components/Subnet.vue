<template>
  <div class="hello">
    <h1>Subnet</h1>
    <div>
      <form>
          <label for="subnetmask1">Enter IP:</label>
        <div class="flex-container">
          <div class="">
            <input @change="validateInput" v-model="maskOctet1" class="form-control" id="subnetmask1"  placeholder="255" :class="{ 'alert-danger':maskOctet1_error}">
          </div>
          <span>.</span>
          <div class="">
            <input @change="validateInput" v-model="maskOctet2" class="form-control" id="subnetmask2"  placeholder="255" :class="{ 'alert-danger':maskOctet2_error}">
          </div>
          <span>.</span>
          <div class="">
            <input @change="validateInput" v-model="maskOctet3" class="form-control" id="subnetmask3"  placeholder="255" :class="{ 'alert-danger':maskOctet3_error}">
          </div>
          <span>.</span>
          <div class="">
            <input @change="validateInput" v-model="maskOctet4" class="form-control" id="subnetmask4"  placeholder="255" :class="{ 'alert-danger':maskOctet4_error}">
          </div>
          <span> / </span>
          <div class="">
            <input @change="validateInput" v-model="network_number" class="form-control" id="network_number"  placeholder="24" :class="{ 'alert-danger':network_number_error}">
          </div>
        </div>

        <div  class="flex-errors">
          <div v-if="show_octetErrormsg" class="error threeThirds">
           <small><i> {{ error_msg }}</i></small>
          </div>
          <div v-if="show_networkNumberErrormsg" class="error third">
            <small><i>{{ network_number_error_msg }}</i></small>
          </div>
        </div>
        <!--<div type="submit" class="cal-btn mt-4" @click="claculateHosts">Calculate</div>-->
        <button v-if="valid_form" type="submit" class="btn btn-primary mt-4" @click="claculateHosts">Calculate</button>
      </form>

      <div class="mt-3">
        <div class="alert alert-info">
          <div v-if="showResult" class="flex-info">
            <div>Mask:</div> 
            <div>{{ decimalMask }}</div>
          </div>

          <div class="flex-info">
            <div>IP: </div>
            <div>{{ maskOctet1 }}.{{ maskOctet2 }}.{{ maskOctet3 }}.{{ maskOctet4 }}</div>
          </div>

          <div v-if="showResult" class="flex-info">
            <div>IP Binary: </div>
            <div>{{ binaryMask }}</div>
          </div>

          <div v-if="showResult" class="flex-info">
            <div>Hosts: </div>
            <div>{{ hosts }}</div>
          </div>

          <!-- <div class="flex-info">
            <div> net Class</div>
            <div>{{ netClass }}</div>
          </div> -->
        </div>
      </div>

      <!-- <div class="mt-3">
        <div class="alert alert-warning">
          <div>{{ test }}</div>
          <div>{{ binary_ip }}</div>
        </div>
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  name: 'Subnet',
  props: {
    msg: String
  },
  data () {
    return {
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
      decimalMask: 'decimal mask',
      test: 'Testing Area',
      error_msg: 'number between 0-255',
      valid_form: false,
      showResult: false,
    }
  },
  methods: {
    claculateHosts (event) {
      event.preventDefault()
      if (this.valid_form) {
        this.binaryMask = this.convertToBinaryWord(this.maskOctet1)
          + '.' + this.convertToBinaryWord(this.maskOctet2)
          + '.' + this.convertToBinaryWord(this.maskOctet3)
          + '.' + this.convertToBinaryWord(this.maskOctet4)
  
        this.getNetworkMask()
        this.getHostsNumber()
        return
      }
      console.log('Unvalid Vlue')
    },
    validateInput(event) {
      if (isNaN(this.network_number) || this.network_number < 0 || this.network_number > 30 || this.network_number.length === 0) {
        this.network_number_error = true
      } else {
        this.network_number_error = false
      }
      
      if (isNaN(this.maskOctet1) || this.maskOctet1 < 0 || this.maskOctet1 > 255 || this.maskOctet1.length === 0) {
        this.maskOctet1_error = true
      } else {
        this.maskOctet1_error = false
      }
      if (isNaN(this.maskOctet2) || this.maskOctet2 < 0 || this.maskOctet2 > 255 || this.maskOctet2.length === 0) {
        this.maskOctet2_error = true
      } else {
        this.maskOctet2_error = false
      }
      if (isNaN(this.maskOctet3) || this.maskOctet3 < 0 || this.maskOctet3 > 255 || this.maskOctet3.length === 0) {
        this.maskOctet3_error = true
      } else {
        this.maskOctet3_error = false
      }
      if (isNaN(this.maskOctet4) || this.maskOctet4 < 0 || this.maskOctet4 > 255 || this.maskOctet4.length === 0) {
        this.maskOctet4_error = true
      } else {
        this.maskOctet4_error = false
      }

      if (this.maskOctet1_error ||
        this.maskOctet2_error ||
        this.maskOctet3_error ||
        this.maskOctet4_error ||
        this.network_number_error
      ){
        this.valid_form = false
        this.showResult = false
      }else{
        this.valid_form = true
        this.showResult = true
      }
    },

    resetValues(){
      this.binary_ip = []
      this.network_number = ''
      // this.network_number_error = false
      this.network_number_error_msg = 'number between 0-30'
      this.maskOctet1 = ''
      // this.maskOctet1_error = false
      this.maskOctet2 = ''
      // this.maskOctet2_error = false
      this.maskOctet3 = ''
      // this.maskOctet3_error = false
      this.maskOctet4 = ''
      // this.maskOctet4_error = false
      this.netClass = 'NetClass'
      this.hosts = 'number of hosts'
      this.binaryMask = 'binary mask'
      this.test = 'Testing Area'
      this.error_msg = 'number between 0-255'
    },

    convertToBinaryWord(value){
      let binary = (value >>> 0).toString(2);
      const length = binary.length
      if (length < 8){
        let zeros = '';
        for (let index = 0; index < 8 - length; index++) {
          zeros += '0';
        }
        zeros = zeros.concat(binary)
        return zeros;
      }
      return binary
    },

    getNetworkMask(){
      let bits_32 = this.convertToBinaryWord(this.maskOctet1)
        + this.convertToBinaryWord(this.maskOctet2)
        + this.convertToBinaryWord(this.maskOctet3)
        + this.convertToBinaryWord(this.maskOctet4)
      const host_bits = bits_32.substr(this.network_number)
      this.test = host_bits
      let mask = '';
      let deci_mask = '';
      let tep_word = '';
      for (let i = 1; i <= (bits_32.length); i++) {
        if (bits_32.length - host_bits.length < i) {
          mask += '0';
          tep_word += '0'
          
          if (i % 8 === 0) {
            deci_mask += parseInt(tep_word, 2) + '.'
            tep_word = ''
          }
        }else{
          mask += '1';
          tep_word += '1'
          if (i % 8 === 0) {
            deci_mask += parseInt(tep_word, 2) + '.'
            tep_word = ''
          }
        }
      }
      console.log('parseint 255: ',mask, deci_mask)
      this.decimalMask = deci_mask.substring(0, deci_mask.length - 1)
    },

    getHostsNumber(){
      let bits_32 = this.convertToBinaryWord(this.maskOctet1)
        + this.convertToBinaryWord(this.maskOctet2)
        + this.convertToBinaryWord(this.maskOctet3)
        + this.convertToBinaryWord(this.maskOctet4)
      const host_bits = bits_32.substr(this.network_number)
      this.test = host_bits
      this.hosts = Math.pow(2, host_bits.length) - 2
    },
  },

  computed: {

    show_octetErrormsg(){
      if (this.maskOctet1_error || this.maskOctet2_error || this.maskOctet3_error || this.maskOctet4_error ){
        return true
      }
      return false
    },
    show_networkNumberErrormsg(){
      if (this.network_number_error){
        return true
      }
      return false
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
  .threeThirds{
    width: 80%;
    margin-right: 2px;
  }
  .third{
    width: 20%;
    margin-left:  auto;
  }

  .error{
    padding: 5px;
    color: crimson;
    border: 1px solid crimson;
    border-radius: 4px;
    text-align: center;

  }

  .flex-info {
    display: flex
  }
  .flex-info > div{
    flex: 2;
    /* border-bottom: 2px solid  #fff; */
    padding: 5px;
  }
  .flex-info:hover{
    /* border-bottom: 2px solid rgb(192, 124, 22); */
    background-color: #fff;
    color: rgb(9, 65, 59);
    border-radius: 3px;
    border-left: 3px solid coral;
  }

</style>
