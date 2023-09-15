<script setup>
//import...
import { ref,computed } from "vue";


// ตัวแปร
const travelList=ref([
  {name: 'ท่าช้าง', price:130, img:"https://img.salehere.co.th/p/1200x0/2023/06/27/qkkm4xhnkttj.jpg", quantity: 0},

  {name: 'GoodView', price:300, img:"https://www.chillnaid.com/wp-content/uploads/2017/08/P8220053-1024x768.jpg", quantity: 0},

  {name: 'RiverSide', price:500, img:"https://scontent.fbkk29-1.fna.fbcdn.net/v/t39.30808-6/307712624_484445423727716_3985616050947085018_n.png?_nc_cat=101&ccb=1-7&_nc_sid=a2f6c7&_nc_eui2=AeHxRe0Hv-tvPThH2EEzBRmEvjWPSkRDNtG-NY9KREM20TQ0xYTqWNnoOxDX5YziC5OEhwdGNYkWM1V_P1Gm7ouK&_nc_ohc=fRukICjihrUAX9tUmz2&_nc_ht=scontent.fbkk29-1.fna&oh=00_AfB2RrV_PuTVOs06rg6-nd-2wJ-PhOQx39BfKNev3NKuvA&oe=6508E7D5", quantity: 0},

  {name: 'WarmUp', price:200, img:"https://siam2nite.media/K19c6rUVbW_LFqSJgu-P9NxQnQ4=/1280x720/smart/locations/1081/cover_large_p19op819ru1v92sof1k0l138rf6p3.jpg", quantity: 0},

  {name: 'สุนทรารมณ์', price:100, img:"https://scontent.fbkk29-4.fna.fbcdn.net/v/t39.30808-6/327442369_874305543617514_7552528986654738604_n.jpg?_nc_cat=110&ccb=1-7&_nc_sid=a2f6c7&_nc_ohc=MoJ_i7SuIaIAX-3D7fu&_nc_ht=scontent.fbkk29-4.fna&oh=00_AfAu7Xc204i-eNoBpHkPYrcgFoBoI2Gzxvw60BKQP_9Qzg&oe=65097AA0", quantity: 0},
  
  {name: 'tamma', price:300, img:"https://www.thai2night.com/upload/shop/photo/_temp/shop_shop_photo_6347_1614263392.jpg", quantity: 0},

]);

//เก็บข้อมูลจองร้าน
const list_local = ref([

]);

// function
function list_local_control(name, quantity) {
  const existingItemIndex = list_local.value.findIndex(item => item.name === name);

  if (existingItemIndex !== -1) {
    list_local.value[existingItemIndex].quantity += quantity;
    list_local.value[existingItemIndex].price = list_local.value[existingItemIndex].quantity * travelList.value.find(item => item.name === name).price;
  } else {
    const item = {
      name: name,
      quantity: quantity,
      price: quantity * travelList.value.find(item => item.name === name).price
    };
    list_local.value.push(item);
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
  list_local.value.splice(index, 1);
}

// PopUp
const form_costumer = ref({
  name: "",
  phone: "",
  date: "",
  time: ""
});

const showPopup = ref(false);

const totalCost = computed(() => {
    return list_local.value.reduce((acc, item) => acc + item.price, 0);
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
    <h1>รายการสินค้า</h1>
  </div><hr>

<div class="container text-center">
  <div class="row">
    <div class="col" v-for="(i,index) in travelList" :key="index">
      <div class="card" style="width: 20rem; height: 40rem;">
        <img :src="i.img" class="card-img-top">
        <div class="card-body">
          <h3 class="card-title">{{ i.name }}</h3>
          <p>ราคาโต๊ะ : {{ i.price }}</p>
          <h6>***จำกัด 6คน/โต๊ะ***</h6>
          <p class="card-text">จำนวนโต๊ะ
            <input type="number" class="out_value" v-model="i.quantity">
          </p>
          <a class="btn btn-primary custom-button" @click="list_local_control(i.name, i.quantity)">จองโต๊ะ</a>
        </div>
      </div>
    </div>
  </div>
</div>  

<h1 style="text-align: center; " v-if="list_local.length>0" class="header">รายการจองโต๊ะ</h1>
<div class="list_cout text-center">
  <table class="table table-hover" v-if="list_local.length >0">
    <thead>
      <tr>
        <th scope="col">ลำดับ</th>
        <th scope="col">ชื่อร้าน</th>
        <th scope="col">จำนวนโต๊ะ</th>
        <th scope="col">ราคา</th>
        <th scope="col"></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(i,index) in list_local" :key="index">
        <th scope="row">{{index+1}}</th>
        <td>{{i.name}}</td>
        <td>
          <div class="quantity-controls">
            <button type="button" class="btn btn-danger" @click="decrementQuantity(i)" :disabled="i.quantity === 0"> - </button>
            <span>{{ i.quantity }}</span>
            <button type="button" class="btn btn-success" @click="incrementQuantity(i)">+</button>
          </div>
        </td>
        <td>{{ i.price }}</td>
        <td>
          <button type="button" class="btn btn-danger" @click="removeItem(index)">ลบรายการ</button>
        </td>
      </tr>
    </tbody>
  </table>
</div>

<div class="contrainer_costumer " v-if="list_local.length>0">
  <div class="form_box"> <h2 style="text-align: center;">กรุณากรอกข้อมูล</h2></div>
  <form >
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
      <label for="time">เวลา </label><br>
      <input type="time" id="time" class="form-control" v-model="form_costumer.time" required>
    </div>
    <div class="form_box">
      <button type="button" class="btn btn-success" @click="togglePopup"> ยืนยันการจอง </button>
    </div>
  </form>
</div>


<div v-if="showPopup" class="overlay">

<div class="popup">
    <div class="card-icon"> <img src="https://cdn-icons-png.flaticon.com/512/5610/5610944.png" alt="" srcset=""> </div>
    <h2 style="text-align: center; color:green ;">ยืนยันการจองสำเร็จ</h2>
    <h3 >รายละเอียด</h3>
    <p><strong>ชื่อ-นามสกุล:</strong> {{ form_costumer.name }}</p>
    <p><strong>เบอร์โทร:</strong> {{ form_costumer.phone }}</p>
    <p><strong>วันที่:</strong> {{ form_costumer.date }}</p>
    <p><strong>เวลา:</strong> {{ form_costumer.time }}</p>
    
    <table class="box_table">
        <thead>
            <tr>
                <th>ลำดับ</th>
                <th>ชื่อร้าน</th>
                <th>จำนวนโต๊ะ</th>
                <th>ราคา</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(i, index) in list_local" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ i.name }}</td>
                <td>{{ i.quantity }}</td>
                <td>{{ i.price }}  บาท </td>
            </tr>
        </tbody>
    </table>

    <p><strong>รวม:</strong> {{ totalCost }} บาท</p>

    <button class="btn btn-danger" @click="showPopup = false">ปิดหน้าต่าง</button>
</div>
</div>


</template>

<style>
*{
  padding: 5px;
}
.header{
  text-align: center;
  background: rgba(47, 29, 207, 0.6);
  padding: 25px;

}


.card-img-top{
  height: 50%;
  padding: 25px;
  background: rgba(31, 111, 216, 0.6);
}

.card-body{
  background: rgba(31, 111, 216, 0.6);
}

.col{
  padding: 10px;
}


.contrainer_customer {
  justify-content: center;
  align-items: center;
  margin-left: 20%;
  margin-right: 20%;

}

.form_box{
    justify-content: center;
    align-items: center;
    padding: 5px;
    margin-left: 30%;
    margin-right: 30%;
    background: rgb(71, 101, 233);
}


.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
}


.popup {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  width: 70%;
  max-height: 80%;
  overflow-y: auto;
}



.card-icon img{
  height: 125px;
  padding: 10px;
  margin-left: 430px;
}

.custom-button{
  background-color: rgba(136, 255, 0, 0.6) !important;
  color: black !important;

}

.custom-button:hover {
  background-color: rgba(68, 126, 2, 0.904) !important; /* สีพื้นหลังของปุ่มเมื่อนำเมาส์มาอยู่บนปุ่ม */
}

.custom-button:active {
  background-color: #D1482E !important  ; /* สีพื้นหลังของปุ่มเมื่อคลิก */
}

.out_value{
  background-color: floralwhite;
  color:black;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 40px; /* ปรับขนาดความกว้างของ input */
  text-align: center; /* จัดข้อความตรงกลาง */
  font-size: 1rem;
}
</style>