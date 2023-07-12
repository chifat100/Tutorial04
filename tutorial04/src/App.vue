<template>
  <div>

    <table >
        <label >Get By Number</label>
        <input class="space" type="text" v-model= "ordernumber" placeholder="enter Number"/>
        <button class="space" @click ="getByNum" >Search By Num</button>
    </table>

    <br>
    <table>
      <label>Delete with ID</label>
      <input type="text"  v-model="id" placeholder="Type your id">
      <button class="space" @click="deletePOST">Click here to delete</button>
    </table>
    <br>

    <table>
        <label class="space">note: Need to refresh after create a POST</label>
    </table>

    <br>

    <table>
        <label >orderNo.</label>
        <input class="space" type="text" v-model ="order.orderNo" placeholder="enter No."/>
        <label >orderType</label>
        <input class="space" type="text" v-model ="order.orderType" placeholder="enter Type"/>
        <label >orderName</label>
        <input class="space" type="text" v-model ="order.orderName" placeholder="enter Name"/>
        <label >orderPrice</label>
        <input class="space" type="text" v-model ="order.orderPrice" placeholder="enter price"/>
        <label >orderAmount</label>
        <input class="space" type="text" v-model ="order.orderAmount" placeholder="enter amount"/>
        <button class="space" @click="createPOST">create a POST</button>
    </table>

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
  
</template>

<script>
  import axios from 'axios'
  import { reactive, ref } from 'vue'

  export default {

    setup() {
    
      const url="http://192.168.20.131:8000/orders";
      const columns = ['orderID','orderNo.', 'orderType', 'orderName','orderPrice','orderAmount','orderDate']

      const items = ref('')
      const id = ref('')
      const ordernumber =ref('')

      const order = {
        "orderNo" : '',
        "orderType" : '',
        "orderName" : '',
        "orderPrice": '',
        "orderAmount": ''
      }


      axios.get(url).then(res => {
        items.value = res.data.payload
        }).catch(error => {
         console.log(error)
      })

      const createPOST = () => {
        axios.post(url, order)
          .then(res => {
            console.log(res)
          })
      } 

      const updatePOST = () => {
        axios.put("http://192.168.20.131:8000/orders/64abcbf470de74001d54cabf",order).then(res => {
          console.log(res)
        }).catch(error => {
          console.log(error)
        })
      }


      const deletePOST = () =>{
          axios.delete(`http://192.168.20.131:8000/orders/${id}`).then(res => {
        }).then(res =>{
          console.log("Deleted post with ID",{id});
        }).catch(error => {
            console.log(id)
        })
    
      }
   
      

    
      return {

        createPOST, 
        updatePOST,
        deletePOST,

        order,
        
        items,
        columns,

        id,
        ordernumber,

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