<script setup>
import { ref } from 'vue'

const drinkData = ref([
  {
    id: '001',
    item: '珍珠奶茶',
    describe: '香濃奶茶搭配QQ珍珠',
    price: 50,
    stock: 20,
    isChecked: false
  },
  {
    id: '002',
    item: '冬瓜檸檬',
    describe: '清新冬瓜配上新鮮檸檬',
    price: 45,
    stock: 18,
    isChecked: false
  },
  {
    id: '003',
    item: '翡翠檸檬',
    describe: '綠茶與檸檬的完美結合',
    price: 55,
    stock: 34,
    isChecked: false
  },
  {
    id: '004',
    item: '四季春茶',
    describe: '香醇四季春茶，回甘無比',
    price: 45,
    stock: 10,
    isChecked: false
  },
  {
    id: '005',
    item: '阿薩姆奶茶',
    describe: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    stock: 25,
    isChecked: false
  },
  {
    id: '006',
    item: '檸檬冰茶',
    describe: '檸檬與冰茶的清新組合',
    price: 45,
    stock: 20,
    isChecked: false
  },
  {
    id: '007',
    item: '芒果綠茶',
    describe: '芒果與綠茶的獨特風味',
    price: 55,
    stock: 18,
    isChecked: false
  },
  {
    id: '008',
    item: '抹茶拿鐵',
    describe: '抹茶與鮮奶的絕配',
    price: 60,
    stock: 20,
    isChecked: false
  }
])

function addCount(e) {
  let IndexNumber = drinkData.value.findIndex((idx) => {
    return idx.id == e.id
  })
  drinkData.value[IndexNumber].stock++
}

function subtCount(e) {
  let IndexNumber = drinkData.value.findIndex((idx) => {
    return idx.id == e.id
  })
  let CurrItem = drinkData.value[IndexNumber]
  if (CurrItem.stock > 0) {
    CurrItem.stock -= 1
  } else {
    return
  }
}

let editContent = ref('')
let btnDisable = ref(false)
let drinkDataV = drinkData.value
let itemCopy

function editItem(e) {
  let IndexNumber = drinkDataV.findIndex((idx) => {
    return idx.id == e.id
  })
  drinkDataV[IndexNumber].isChecked = true
  btnDisable.value = true
  itemCopy = { ...drinkDataV[IndexNumber] }
  editContent.value = itemCopy.item
}

function cancelItem(e) {
  let IndexNumber = drinkDataV.findIndex((idx) => {
    return idx.id == e.id
  })
  drinkDataV[IndexNumber].isChecked = false
  btnDisable.value = false
}

function OKItem(e) {
  let IndexNumber = drinkDataV.findIndex((idx) => {
    return idx.id == e.id
  })
  if (editContent.value != '') {
    drinkDataV[IndexNumber].item = editContent.value
    drinkDataV[IndexNumber].isChecked = false
    btnDisable.value = false
  } else {
    alert('請輸入內容')
  }

  // console.log(editContent.value)
}
</script>

<template>
  <div class="row justify-content-center mt-5">
    <div class="col-md-8 col-xl-6">
      <table class="table text-center">
        <thead class="table-dark">
          <tr>
            <th scope="col"></th>
            <th scope="col">品項</th>
            <th scope="col">描述</th>
            <th scope="col">價格</th>
            <th scope="col">庫存</th>
          </tr>
        </thead>
        <tbody v-for="(item, index) in drinkData" v-bind:key="index.id">
          <tr :class="{ 'table-light': index % 2 === 0 }">
            <td class="editWidth">
              <div :class="{ inputIn: item.isChecked }">
                <div class="inputArea input-group">
                  <input type="text" v-model="editContent" class="form-control" />
                  <button type="button" @click="OKItem(item)" class="btn btn-outline-secondary">
                    確認
                  </button>
                  <button type="button" @click="cancelItem(item)" class="btn btn-outline-danger">
                    取消
                  </button>
                </div>
                <button
                  type="button"
                  class="editbtn btn btn-outline-secondary"
                  @click="editItem(item)"
                  :disabled="btnDisable"
                >
                  編輯品項
                </button>
              </div>
            </td>
            <td class="align-middle">{{ item.item }}</td>
            <td class="align-middle">
              <small>{{ item.describe }}</small>
            </td>
            <td>{{ item.price }}</td>
            <td>
              <button type="button" class="btn btn-secondary btn-sm" v-on:click="subtCount(item)">
                -
              </button>
              {{ item.stock }}
              <button type="button" class="btn btn-secondary btn-sm" v-on:click="addCount(item)">
                +
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.inputArea {
  display: none;
}
.inputIn .inputArea {
  display: flex;
}
.inputIn .editbtn {
  display: none;
}
.editWidth {
  width: 300px;
}
@media (max-width: 1400px) {
  .editWidth {
    width: 230px;
  }
}
@media (max-width: 1200px) {
  .editWidth {
    width: 200px;
  }
}
@media (max-width: 768px) {
  .editWidth {
    width: 100px;
  }
}
</style>
