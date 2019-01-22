<template>
  <div class="home">
    <HelloWorld :source-left="data1"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    HelloWorld
  },
  data () {
  	return {
			data1: [
	      {id: 1,value: 'UNYB-line1290', isSelect: false},
	      {id: 2,value: 'UOLB-line0', isSelect: false},
	      {id: 3,value: 'HBYB-lidfne585', isSelect: false},
	      {id: 4,value: 'UNYB-lwe560', isSelect: false},
	      {id: 5,value: 'DFLKYB-ldne3290', isSelect: false},
		  ],
		  data2:[{id: 6,value: 'SRESB-line1290', isSelect: false}],
  	}
  }
}
</script>
