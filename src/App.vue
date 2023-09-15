<script setup>
// import . . .
import {ref,computed} from "vue";
//ตัวแปร
const travel = ref("");
const money =ref(0);

//const travelList =ref (['เกาะช้าง','เกาะสีชัง','เกาะกูด','เกาะล้าน','เกาะพีพี'])
const travelList = ref ([
{name:'Forest_Bake', price : 60 , img : "img/Forest_Bake.png" } ,
{name:'Naturetalk', price : 60, img :"img/Naturetalk.png"}, 
{name:'123_Glamp' , price :80, img :"img/123_Glamp.png"},
{name:'Magokoro', price : 80, img :"img/Magokoro_Teahouse.png"},
{name:'Lamour_cafe ' , price : 40, img :"img/Lamour_cafe.png"},
{name:'Yelloo ' , price : 80, img :"img/Yelloo.png"}
]);

//เก็บ list การจอง 
const bookingList = ref([]);


//function
function list_local_control(name, quantity) {
  const existingItemIndex = bookingList.value.findIndex((item) => item.name === name);

  if (existingItemIndex !== -1) {
    bookingList.value[existingItemIndex].quantity += quantity;
    bookingList.value[existingItemIndex].price =
      bookingList.value[existingItemIndex].quantity *
      travelList.value.find((item) => item.name === name).price;
  } else {
    const item = {
      name: name,
      quantity: quantity,
      price: quantity * travelList.value.find((item) => item.name === name).price,
    };
    bookingList.value.push(item);
  }
}

function incrementQuantity(item) {
  item.quantity++;
  item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
}

function decrementQuantity(item) {
  if (item.quantity > 0) {
    item.quantity--;
    item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
  }
}

function removeItem(index) {
  bookingList.value.splice(index, 1);
}



//pop up

const form_costumer = ref({
  name: "",
  phone: "",
  date: "",
  time: ""
});

const showPopup = ref(false);

const totalCost = computed(() => {
    return bookingList.value.reduce((acc, item) => acc + item.price, 0);
});


function togglePopup() {
  showPopup.value = !showPopup.value;
}


</script>



<template>

  <HelloWorld v-if="showPopup" />
  <div class="overlay" v-if="showPopup">
    <HelloWorld />
  </div>

  <div class="header">
    <H1>Cafe in Chiang Mai</H1>
  </div><hr>

<div class="card1">
  <div class="container text-center">
  <div class="row">
    <div class="col-6" v-for="(i, index) in travelList" :key="index">
      <div class="card mb-4">
        <img :src="i.img" class="card-img-top card-img">
        <div class="card-body text-center">
          <h3 class="card-title">{{ i.name }}</h3>
          <p>ราคาการจองเข้า : {{i.price}}</p>
   
          <p class="card-text">จำนวนโต๊ะ กรุณากรอก
            <input type="number" class="cout_value" v-model="i.quantity">
          </p>
          <a class="btn btn-primary" @click="list_local_control(i.name, i.quantity)" > จองโต๊ะ </a>
        </div>
      </div>
    </div>
  </div>
</div>
</div>
 

 
    <h1 style="text-align: center;" class="header">รายการจองโต๊ะ</h1>
    <table class="table table-hover">
      <thead>
        <tr>
          <th scope="col">ร้านที่</th>
          <th scope="col">ชื่อร้าน</th>
          <th scope="col">จำนวนโต๊ะ</th>
          <th scope="col">ราคา</th>
          <th scope="col"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(i, index) in bookingList" :key="index">
          <th scope="row">{{ index + 1 }}</th>
          <td>{{ i.name }}</td>
          <td>
          <div class="add">  
            <div class="quantity-controls">
              <button type="button" class="btn btn-danger" @click="decrementQuantity(i)" :disabled="i.quantity === 0"> - </button>
              <span>{{ i.quantity }}</span>
              
            </div>
          </div>  
          </td>
          <td>{{ i.price }}</td>
          <td>
            <button type="button" class="btn btn-danger" @click="removeItem(index)">ลบรายการ</button>
          </td>
        </tr>
      </tbody>
    </table>


  
<div class="contrainer_costumer ">
  <div class="form_box"> <h2 style="text-align: center;">กรุณากรอกข้อมูลคนจอง</h2></div>
  <form>
    <div class="form_box">
      <label for="name">ชื่อ-นามสกุล </label><br>
      <input type="text" id="name" class="form-control" v-model="form_costumer.name" required>
    </div>
    <div class="form_box">
      <label for="phone">เบอร์โทร </label><br>
      <input type="tel" id="phone" class="form-control" v-model="form_costumer.phone" required>
    </div>
    <div class="form_box">
      <label for="date">วันที่ </label><br>
      <input type="date" id="date" class="form-control" v-model="form_costumer.date" required>
    </div>
    <div class="form_box">
      <label for="time">เวลา ที่ต้องการเข้า</label><br>
      <input type="time" id="time" class="form-control" v-model="form_costumer.time" required>
    </div>
    
  </form>
</div>




  <div class="popup">
      <h2 style="text-align: center; color:rgb(255, 255, 255) ;">รายละเอียดการจอง</h2>
      <h3 >ข้อมูลลูกค้า</h3>
      <p><strong>ชื่อ-นามสกุล:</strong> {{ form_costumer.name }}</p>
      <p><strong>เบอร์โทร:</strong> {{ form_costumer.phone }}</p>
      <p><strong>วันที่:</strong> {{ form_costumer.date }}</p>
      <p><strong>เวลา:</strong> {{ form_costumer.time }}</p>
      
      <table class="box_table">
          <thead>
              <tr>
                  <th>ลำดับ</th> <th>ชื่อร้าน</th> <th>จำนวนโต๊ะ</th> <th>ราคา</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="(i, index) in bookingList" :key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ i.name }}</td>
                  <td>{{ i.quantity }}</td>
                  <td>{{ i.price }}  บาท </td>
              </tr>
          </tbody>
      </table>

      <p><strong>รวมราคาที่ต้องชำระ:</strong> {{ totalCost }} บาท</p>

      <div class="form_box">
      <button type="button" class="btn btn-success" @click="togglePopup"> ยืนยันการจอง </button>
    </div>
  </div>




</template>


<style>

body{
  background-image: url(https://i.pinimg.com/564x/49/e0/ee/49e0ee84c3b5f2b3228a09bbe8ea0946.jpg);


}
.header{
  background: #412626;
  text-align: center;
  
  font-family: fantasy;
  color: #fff;
  


}

.card {
  
  border: 3px solid #ddd;
  background-color: #0000008f;
  color: #fff;
}
.card-img-top{
  height: 100%;
  padding: 25px;
}

.col{
  padding: 10px;
}




.form_box,.popup{
    justify-content: center;
    align-items: center;
    padding: 10px;
    margin-left: 20%;
    margin-right: 20%;
    background: #401a1ac8;
    color: #fff;
    
    
}


.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  
  display: flex;
  justify-content: center;
  align-items: center;
}




.header{
  padding: 25px;
}

.card-icon img{
  height: 125px;
  padding: 10px;
  margin-left: 430px;
}
.table-container {
  display: flex;
  justify-content: center;
}
table {
  width: 20%; /* ปรับความกว้างของตารางตามที่คุณต้องการ */
  border-collapse: collapse;
  margin-top: 20px;
}
table, th, td {
  border: 1px solid #ccc;
  text-align: center;
}
th, td {
  padding: 10px;
}




</style>