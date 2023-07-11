<template>
  <div>
  <div>
    <table >
      <label >Get By Number</label>
      <input class="space" type="text" v-model= "get_num" placeholder="enter Number"/>
      <button class="space" @click ="getByNum" >Search By Num</button>
    </table>

    <br>
    <label>Refresh</label> <button class="space" @click="reFresh">Update</button>
    <br>
    <table>
      <thead>
        <tr>
          <th v-for="column in columns" :key="column">{{ column }}</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(row, index) in items" :key="index">
          <td v-for="(value, key) in row" :key="key">{{ value }}</td>
        </tr>
      </tbody>

      <label>Note:</label>
    </table>
  </div>

  <br>

  <div>
    <table>
      <label >orderNo.</label>
      <input class="space" type="text" v-model ="set_no" placeholder="enter No."/>
      <label >orderType</label>
      <input class="space" type="text" v-model ="set_yype" placeholder="enter Type"/>
      <label >orderName</label>
      <input class="space" type="text" v-model ="set_name" placeholder="enter Name"/>
      <label >orderPrice</label>
      <input class="space" type="text" v-model ="set_price" placeholder="enter price"/>
      <label >orderAmount</label>
      <input class="space" type="text" v-model ="set_amount" placeholder="enter amount"/>
      <button class="space" @click="createPOST">create a POST</button>
    </table>
  </div>
</div>
  


</template>

<script>
import axios from 'axios'
import { ref } from 'vue'

export default {
   
  setup() {
    
    const url="http://192.168.20.131:8000/orders";
    const columns = ['orderID','orderNo.', 'orderType', 'orderName','orderPrice','orderAmount','orderDate']

    const items = ref('')
    const numItem = ref('')

    const get_num = ref('')

    const createPOST = () => {
      axios.post(url, order)
        .then(res => {
          console.log(res)
        })
    }

    const reFresh = () => {
      axios.put("http://192.168.20.131:8000/orders/64abcbf470de74001d54cabf",order).then(res => {
        console.log(res)
      }).catch(error => {
        console.log(error)
      })

      this.$router.push({
        path: './src/components/reload',
        name:'reload'
      })
    }


    const getByNum = () =>{
        var  num = get_num;
        axios.get(url,{
          params: num
        }).then(res =>{
          numItem.value = res.data.payload
        }).then(res => {console.log})
        
    }
    

    var order = {
      "orderNo" : "05",
      "orderType" : "pistol,vapor",
      "orderName" : "Nobody",
      "orderPrice": "799",
      "orderAmount": "4"
    }
    
    axios.get(url).then(res => {
      items.value = res.data.payload
    }).catch(error => {
        console.log(error)
      })

    /* axios.delete(url).then(res => {
      items.value = res.data.payload
    }).catch(error => {
        console.log(error)
      }) */
    
      

    
    return {

      reFresh,
      createPOST, 
      getByNum,

      items,
      columns,

      get_num,
      numItem,


    };
  },

}
</script>

<style>
table, th, td {
  border: 1px solid black;
}

.space {
  display: inline-block;
  margin-left: 10px;
}
</style>