<template>
  <div class="home">
    <HelloWorld :source-l="data1" :target-r="data2"/>
    <Main :source-left="data3" :target-right="data4" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import Main from '@/components/main.vue'
export default {
  name: 'home',
  components: {
    HelloWorld,
    Main
  },
  data () {
  	return {
			data1: [
	      {id: 1,value: 'UNYB-line1290', isSelect: false},
	      {id: 2,value: 'UOLB-line0', isSelect: false},
	      {id: 3,value: 'HBYB-lidfne585', isSelect: false},
	      {id: 4,value: 'UNYB-lwe560', isSelect: false},
	      {id: 5,value: 'DFLKYB-ldne3290', isSelect: false}
		  ],
		  data2:[{id: 6,value: 'SRESB-line1290', isSelect: false}],
      data3:[
        {key: 1, label: 'nihaoadf'},
        {key: 2, label: 'nihaasdfo'},
        {key: 3, label: 'nzxcvihao'},
        {key: 4, label: 'niasdfhao'},
        {key: 5, label: 'nihao'},
        {key: 6, label: 'niasdfao'},
        {key: 7, label: 'nihao'},
        {key: 8, label: 'nizxvchao'},
        {key: 9, label: 'nizxcvhao'}
      ],
      data4:[
        {key: 10, label: 'nzcxvihao'},
        {key: 11, label: 'nzxcvhao'}
      ]
  	}
  }
}
</script>
