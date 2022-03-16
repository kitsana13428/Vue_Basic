<template>
  <section>
    <img v-bind:src="picture" v-bind:width="size" v-bind:height="size"> <!--การใช้ v-bind ผูกกับข้อมูล-->
    <h1>ชื่อ : {{firstName}}</h1>
    <h1>ชื่อ นามสกุล : {{getFullName()}}</h1> <!--ดึงค่าจากฟังก์ชั่น-->
    <h1>อายุ : {{age}}</h1>
    <h1>ที่อยู่ : <span v-html="address"></span></h1> <!--ทำให้ tag html แสดงผลได้ด้วย v-html-->
    <p>Social : <a :href="social" trget="_blank">w3schools</a></p>
    <p>ตัวเลข :</p> <!--การใช้งานอาเรย์-->
    <ul>
      <li>{{number[0]}}</li>
      <li>{{number[1]}}</li>
      <li>{{number[2]}}</li>
    </ul>

    <p>ข้อมูลพิ้นฐาน :</p> <!--การใช้งานออฟเจก-->
    <ul>
      <li>เพศ : {{general.gender}}</li>
      <li>ส่วนสูง : {{general.weight}}</li>
      <li>น้ำหนัก : {{general.height}}</li>
    </ul>
    <button v-on:click="showData">ตกลง</button> <!--การใช้งาน Event on click-->
    <button @click="incerment(10)">เพิ่ม</button> <!--การเพิ่มค่าด้วย on click-->
    <button @click="decrement">ลด</button>
  </section>
</template>

<script>


export default {
  name: 'App',
 data(){
   return{
     firstName:"Kitsana",
     lastName:"Rattanapholsaen",
     age:25,
     address:"<i>กรุงเทพมหานคร</i>",
     picture:"data:image/png;base64,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"
    ,size:100,
    social: "https://www.w3schools.com/?fbclid=IwAR2bHpaSP2DNIEo5XGqIhBX6lK22OIlxIZpP6yhiMe8tPWZ25lmngCV564Q",
    number:["0","1","2"] ,
    general:{gender:"ชาย",weight:65,height:170},
   }

 },
 methods:{
   getFullName(){
     return `${this.firstName +" "+ this.lastName}`
   },
   showData(){
     alert(this.firstName)
   },
   incerment(value){ //การส่ง อากิวเม้นมาคำนวณ
     this.age+=value
   },
   decrement(){
     this.age--
   }
 }
}
</script>

<style>

</style>
