<template>
  <div class="container">
    <h1>Subnet Calculations</h1>
    <Subnet />
  </div>
</template>

<script>
    // @ is an alias to /src
import Subnet from '../components/Subnet.vue'
    export default {
        name: 'SubnetCalculate',
        components: {
            Subnet
        }
    }
</script>
