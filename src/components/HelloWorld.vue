<template>
<div id="box">
  <div class="box">
    <div class="title">
      <span :class="{active: leftSelect === data1.length}" @click="selectAll(data1)">课件列表</span>
      <span>{{leftSelect ? leftSelect : 0}}/{{data1.length}}</span>
    </div>

    <div class="search_parent">
      <!-- <div class="search_div"> -->
        <input class="search_input" v-model="sourceData" :class="heightLight ? 'search_input_focus' : ''" type="text" @focus="heightLight=true" @blur="heightLight=false" placeholder="请输入课件级别/编号">
      <!-- </div> -->
    </div>

    <div @drop="dropPub($event, 2)" @dragover.prevent class="left">
      <div
        @drag="ondrag"
        @dragstart="dragstart($event, item)"
        @dragend="dragend"
        draggable="true"
        class="hover-color"
        :class="{active: item.isSelect }" 
        v-on:click="hanldleClick(data1, item.id)" 
        v-for="item in changeSourceData" 
        :key="item.id">{{item.value}}</div>
    </div>
  </div>
  
  <div class="middle">
    <div v-on:click="moveItem(data2,data1)" :class="{active: rightSelect > 0}">移除</div>
    <div v-on:click="moveItem(data1, data2)" :class="{active: leftSelect > 0}">移入</div>
  </div>

  <div class="box">
    <div class="title">
      <span :class="{active: rightSelect === data2.length}" @click="selectAll(data2)">已选课件</span>
      <span>{{rightSelect ? rightSelect : 0}}/{{data2.length}}</span>
    </div>

    <div class="search_parent">
      <!-- <div class="search_div"> -->
        <input class="search_input" v-model="targetData" :class="heightLight1 ? 'search_input_focus' : ''" @focus="heightLight1=true" @blur="heightLight1=false" type="text" placeholder="请输入课件级别/编号">
      <!-- </div> -->
    </div>
    
    <div @drop="dropPub($event, 1)" @dragover.prevent class="right">
      <div
      class="hover-color"
      @drag="ondrag" 
      @dragstart="dragstart($event, item)"
      @dragend="dragend"
      draggable="true" 
      :class="{active: item.isSelect }" 
      v-on:click="hanldleClick(data2, item.id)" 
      v-for="item in changeTargetData" 
      :key="item.id">{{item.value}}</div>
    </div>
  </div>
</div>
</template>
<script>
export default {
data () {
  return {
    heightLight: false,
    heightLight1: false,
    data1: [
      {id: 1,value: 'UNYB-line1290', isSelect: false},
      {id: 2,value: 'UOLB-line0', isSelect: false},
      {id: 3,value: 'HBYB-lidfne585', isSelect: false},
      {id: 4,value: 'UNYB-lwe560', isSelect: false},
      {id: 5,value: 'DFLKYB-ldne3290', isSelect: false},
    ],
    data2:[{id: 6,value: 'SRESB-line1290', isSelect: false}],
    target: {
      y: '',
      id: ''
    },
    sourceData: '',
    targetData: '',
    changeSourceData: [],
    changeTargetData: []
  }
},
created () {
  this.changeSourceData = this.data1
  this.changeTargetData = this.data2
},
watch: {
  data2 () {
    this.changeTargetData = this.data2
  },
  sourceData (val) {
    let emptyArr = []
    if (val) {
      // val = val.toLowerCase()
      for (let i = 0; i < this.data1.length; i++) {
        if (this.data1[i].value.indexOf(val) != -1) {
          emptyArr.push(this.data1[i])
        }
      }
      this.changeSourceData = emptyArr
    } else {
      this.changeSourceData = this.data1
    }
  },
  targetData (val) {
    let emptyArr = []
    if (val) {
      // val = val.toLowerCase()
      for (let i = 0; i < this.data2.length; i++) {
        if (this.data2[i].value.indexOf(val) != -1) {
          emptyArr.push(this.data2[i])
        }
      }
      this.changeTargetData = emptyArr
    } else {
      this.changeTargetData = this.data2
    }
  }
},
methods: {
  ondrag(event) {
    this.target.y = event.y
  },
  dropPub (event, type) {
    if (type == 1) {
      this.drop(event,this.data1, this.data2)
    } else {
      this.drop(event,this.data2, this.data1)
    }
  },
  drop (event,handle, target) {            
    let id = event.dataTransfer.getData('id')
    handle.forEach((item,index)=>{
      if (item.id == id) {
        let temp = handle.splice(index,1)
        temp[0].isSelect = false
        target.push(temp[0])
      }
    })
    this.resort(event, target)    
  },
  resort (event,target) {
    let eles =event.currentTarget.children
    console.log(eles, target)            
    for (let i = 0; i < target.length; i++) {
      if (target[i].id == this.target.id) {
        target[i].y = this.target.y
      } else {
        target[i].y = eles[i].getBoundingClientRect().y
      }
    }
    for (let i = 0; i < target.length-1; i ++) {
      for (let j = 0; j < target.length-i-1; j++ ) {
        if (target[j].y > target[j+1].y) {
          let swap = target[j]
          target[j] = target[j+1]
          target[j+1] = swap
        }
      }
    }
    this.$forceUpdate()
  },
  dragstart (event,item) {
    this.target.id = item.id
    event.dataTransfer.setData('id', item.id)
  },
  dragend (event) {
    event.dataTransfer.clearData()
  },
  hanldleClick (data,id) {
    data.forEach(element => {
      if (element.id == id) {
        element.isSelect = !element.isSelect
      }
    });
  },
  moveItem (handle, target) {
      let temp = handle.filter(item=> {
          return item.isSelect == true
      })
      if (temp.length <=0) return false
      temp.forEach(item=> {
          let index = handle.indexOf(item)
          handle.splice(index,1)
          item.isSelect = false
          target.push(item)
      })
  },
  selectAll (data) {
    let isSelectAll = data.every(item => {
      return item.isSelect == true
    })
    data.forEach(item=> {
      if (isSelectAll) {
        item.isSelect = false
      } else {
        item.isSelect = true
      }
    })
  },
  sourceSearch (val) {
    console.log(val.target.value)
    
  },
  targetSearch (val) {
    console.log(val.target.value)
  }
},
computed: {
  leftSelect () {
    if (this.data1.length === 0) return false
    let arr =  this.data1.filter(item=> {
      return item.isSelect == true
    })
    return arr.length
  },
  rightSelect () {
    if (this.data2.length == 0) return false
    let arr =  this.data2.filter(item=> {
      return item.isSelect == true
    })
    return arr.length
  }
}
}
</script>
<style>
  * {
    margin: 0;
    padding: 0;
  }
  #box {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .box {
    border: 1px solid rgb(235, 238, 245);
  }
  .box .title {
    font-size: 14px;
    box-sizing: border-box;
    padding-right: 10px;
    display: flex;
    justify-content: space-between;
    width: 200px;
    height: 40px;
    line-height: 40px;
    background: #f5f7fa;
  }
  .box .title span:first-child {
    display: inline-block;
    background: #f5f7fa;
    background-image: url(../../public/check-box-outline-blank.png);
    background-repeat: no-repeat;
    background-position: 10px center;
    background-size: 20px 20px;
    padding-left: 35px;
    cursor: pointer;
  }
  .box .title span:first-child.active {
    background-image: url(../../public/check_box.png);
  }
  .left, .right {
    width: 200px;
    height: 250px;
    padding: 5px 0;
    box-sizing: border-box;
    overflow-y: scroll;
    overflow-x:hidden;
  }
  .left >div , .right > div {
    cursor: pointer;
    text-align: left;
    background-image: url(../../public/check-box-outline-blank.png);
    background-repeat: no-repeat;
    background-position: 10px center;
    background-size: 20px 20px;
    font-size: 14px;
    padding-left: 35px;
    padding-top: 5px;
    padding-bottom: 5px;
  }
  .left > div.active , .right > div.active {
    background-image: url(../../public/check_box.png);
  }
  .left > div.hover-color:hover, .right > div.hover-color:hover {
    color: rgb(64, 158, 255);
  }
  .middle {
    margin:0 40px;
  }
  .middle > div {
    margin: 10px 0;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 1px solid #dcdfe6;
    font-size: 12px;
    background-color: #f5f7fa;
    color: #c0c4cc;
  }
  .middle > div.active {
    background-color: #409eff;
    border-color: #409eff;
    color: #ffffff;
  }
  .search_parent {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 20px 0;
  }
  /* .search_div {
    border: 1px solid #dddddd;
    border-radius: 20px;
    width: 150px;
    height: 30px;
    padding-left: 10px;
  } */
  .search_input {
    width: 150px;
    height: 10px;
    border-radius: 50px;
    border: 1px solid #ccc;
    outline: none;
    padding: 10px 0px 10px 10px;
    font-size: 14px;
  }
  .search_input_focus {
    border: 1px solid #409eff;
  }
  ::-webkit-input-placeholder { /* WebKit, Blink, Edge */
    color: rgb(192, 196, 204);
  }
  :-moz-placeholder { /* Mozilla Firefox 4 to 18 */
    color: rgb(192, 196, 204);
  }
  ::-moz-placeholder { /* Mozilla Firefox 19+ */
    color: rgb(192, 196, 204);
  }
  :-ms-input-placeholder { /* Internet Explorer 10-11 */
    color: rgb(192, 196, 204);
  }
</style>
