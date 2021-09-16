<template>
  <div id="app">
    <div class="container-head">
      <div class="container-title">
        <div class="title" @click="selected = 1">Nhập thành viên mới</div>
        <div class="title" @click="selected = 2">Xem danh sách thành viên</div>
        <div class="title" @click="selected = 3">Sửa thông tin thành viên</div>
        <div class="title" @click="selected = 4">Xóa thành viên</div>
      </div>
      <div class="containre-content">
        <div v-show="selected == 1" class="form-add-member">
          <h2>Nhập thông tin thành viên</h2>
          <div>
            <div>
              <label for="add-member-name">Tên</label>
              <input type="text" id="add-member-name" name="" v-model="newMember.name">
            </div>
            <div>
              <label for="add-member-gender">Giới tính</label>
              <input type="text" id="add-member-gender" name="" v-model="newMember.gender">
            </div>
            <div>
              <label for="add-member-status">Tình trạng</label>
              <input type="text" id="add-member-status" name="" v-model="newMember.status">
            </div>
            <div>
              <button @click="addMember">Hoàn thành</button>
            </div>
          </div>
        </div>
        <div v-show="selected == 2" class="Watch-all-members">
          <h2>Xem danh sách thành viên</h2>
          <table>
            <tr>
              <th>id</th>
              <th>Tên</th>
              <th>Giới tính</th> 
              <th>Tình trạng</th>
            </tr>
            <tr v-for="(member, index) in members" :key="index">
              <td>{{member.id}}</td>
              <td>{{member.name}}</td>
              <td>{{member.gender}}</td>
              <td>{{member.status}}</td>
            </tr>
          </table>
        </div>
        <div v-show="selected == 3" class="Update-a-member">
          <h2>Cập nhật thành viên</h2>
          <div>
            <div>
              <label for="name">iD</label>
              <input type="text" id="name" name="" v-model="updateDataMember.id">
            </div>
            <div>
              <label for="name">Tên cần sửa</label>
              <input type="text" id="name" name="" v-model="updateDataMember.name">
            </div>
            <div>
              <label for="gender">Giới tính cần sửa</label>
              <input type="text" id="gender" name="" v-model="updateDataMember.gender">
            </div>
            <div>
              <label for="status">Tình trạng cần sửa</label>
              <input type="text" id="status" name="" v-model="updateDataMember.status">
            </div>
            <div>
              <button @click="updateMember">Hoàn thành</button>
            </div>
          </div>
        </div>
        <div v-show="selected == 4" class="Delete-a-member">
          <h2>Xóa thành viên</h2>
            <div>
              <label for="deleteID">ID Cần xóa:</label>
              <input type="text" id="deleteID" name="" v-model="deleteID">
            </div>
            <div>
              <button @click="deleteMember">Hoàn thành</button>
            </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const baseURL = 'http://localhost:3000/listMember';
import axios from 'axios';
export default {
  name: 'App',
  data(){
    return {
      selected: 1,
      members: Array,
      newMember: {
        name: "",
        gender:"",
        status:""
      },
      updateDataMember: {
        id: "",
        name: "",
        gender:"",
        status:""
      },
      deleteID: ""
    }
  },
  methods: {
    async getData(){
      try {
        const res = await axios.get(baseURL)

        this.members = res.data;
      } catch(e) {
        console.error(e)
      }
    },
    async addMember() {
      const res = await axios.post(baseURL, this.newMember)
      this.members = [...this.members, res.data]
      this.newMember.name=""
      this.newMember.gender=""
      this.newMember.status=""

    },
    async updateMember() {
      await axios.patch(baseURL + "/" + this.updateDataMember.id, this.updateDataMember)
      this.getData()
      this.updateDataMember.id=""
      this.updateDataMember.name=""
      this.updateDataMember.gender=""
      this.updateDataMember.status=""
    },
    async deleteMember() {
       await axios.delete(baseURL + "/" + this.deleteID)
        this.getData()
    }
  },
  created() {
    this.getData();
  }
}
</script>

<style>
label {
  display: inline-block;
  width: 80px;
  text-align: center;
}
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  display: inline-block;
  min-width: 194px;
  text-align: center;
}
table {
  width: 100%;
}
.container-head {
  width: 800px;
  margin: 0 auto;
}
.container-title {
  display: flex;
}
.container-title .title {
  height: 30px;
  line-height: 30px;
  font-size: 20px;

  flex-grow: 1;
  border: 1px solid red;
  text-align: center;
  cursor: pointer;
  border-radius: 5px;
}
.container-title .title:hover {
  background-color: #ebebeb;
}
.containre-content {
  margin-top: 5px;
  border: 1px solid red;
}
.Update-a-member label,
.Delete-a-member label {
  width: 150px;
}

</style>
