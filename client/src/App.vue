<template>
  <div>
    <!-- Bài 3 -->
    <ListProduct />
    <br />
    <main>
      <div class="header-main">
        <h2>Manager Product</h2>
        <div class="flex gap-2">
          <select name="" id="" @change="(e) => handleSortTime(e.target.value)">
            <option value="">Chọn thời gian</option>
            <option value="desc">Gần nhất</option>
            <option value="asc">Xa nhất</option>
          </select>
          <button class="btn-add" @click="handleShowForm">
            Add New Product
          </button>
        </div>
      </div>
      <table>
        <thead>
          <tr>
            <th>STT</th>
            <th>Image</th>
            <th>Name</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>CreatedAt</th>
            <th>Option</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product, index) in products" :key="product.id">
            <td>{{ index + 1 }}</td>
            <td>
              <img :src="product.image" alt="Orange" />
            </td>
            <td>{{ product.name }}</td>
            <td>{{ product.price }} đ</td>
            <td>{{ product.quantity }}</td>
            <td>{{ product.createdAt.split("-").reverse().join("/") }}</td>
            <td>
              <button class="btn-edit" @click="handleShowEdit(product)">
                Edit
              </button>
              <button class="btn-delete" @click="handleDelete(product.id)">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </main>

    <div
      v-if="showForm"
      class="fixed inset-0 flex justify-center items-center bg-black bg-opacity-50"
    >
      <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-md">
        <h3 class="text-xl font-semibold mb-4">Thêm thông tin sản phẩm</h3>
        <form @submit.prevent="handleSubmit">
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Tên sản phẩm</label>
            <input
              type="text"
              v-model="inputValue.name"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.nameProduct }}</p> -->
          </div>
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Hình ảnh</label>
            <input
              type="text"
              v-model="inputValue.image"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.image }}</p> -->
          </div>
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Giá</label>
            <input
              type="number"
              min="10000"
              v-model="inputValue.price"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.price }}</p> -->
          </div>
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Số lượng</label>
            <input
              type="number"
              min="1"
              max="100"
              v-model="inputValue.quantity"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.quantity }}</p> -->
          </div>
          <div class="flex justify-end gap-2">
            <button
              type="button"
              class="bg-red-500 hover:bg-red-600 text-white font-semibold py-2 px-4 rounded-lg"
              @click="handleCloseForm"
            >
              Đóng
            </button>

            <button
              type="submit"
              class="bg-green-500 hover:bg-green-600 text-white font-semibold py-2 px-4 rounded-lg"
            >
              Lưu thông tin
            </button>
          </div>
        </form>
      </div>
    </div>

    <div
      v-if="showFormEdit"
      class="fixed inset-0 flex justify-center items-center bg-black bg-opacity-50"
    >
      <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-md">
        <h3 class="text-xl font-semibold mb-4">Sửa thông tin sản phẩm</h3>
        <form @submit.prevent="handleUpdate">
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Tên sản phẩm</label>
            <input
              type="text"
              v-model="productEdit.name"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.nameProduct }}</p> -->
          </div>
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Hình ảnh</label>
            <input
              type="text"
              v-model="productEdit.image"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.image }}</p> -->
          </div>
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Giá</label>
            <input
              type="number"
              min="10000"
              v-model="productEdit.price"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.price }}</p> -->
          </div>
          <div class="mb-4">
            <label class="block text-sm font-medium mb-2">Số lượng</label>
            <input
              type="number"
              min="1"
              max="100"
              v-model="productEdit.quantity"
              class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <!-- <p class="text-red-500">{{ error.quantity }}</p> -->
          </div>
          <div class="flex justify-end gap-2">
            <button
              type="button"
              class="bg-red-500 hover:bg-red-600 text-white font-semibold py-2 px-4 rounded-lg"
              @click="handleCloseFormEdit"
            >
              Đóng
            </button>

            <button
              type="submit"
              class="bg-green-500 hover:bg-green-600 text-white font-semibold py-2 px-4 rounded-lg"
            >
              Cập nhật
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script setup>
import { reactive, ref } from "vue";
import ListProduct from "./ListProduct.vue";
import { format } from "date-fns";
const showForm = ref(false);
const showFormEdit = ref(false);
const products = ref([]);
const productEdit = ref(null);

const inputValue = reactive({
  name: "",
  image: "",
  price: 0,
  quantity: 1,
});

const handleShowForm = () => {
  showForm.value = true;
};

const handleCloseForm = () => {
  showForm.value = false;
};
// Bài 2
const getAllProduct = async () => {
  const data = await fetch("http://localhost:8080/products");
  const res = await data.json();
  console.log(res);
  products.value = res;
};
getAllProduct();

// Hàm thêm sản phẩm
const handleSubmit = async () => {
  const data = {
    name: inputValue.name,
    image: inputValue.image,
    price: inputValue.price,
    quantity: inputValue.quantity,
    createdAt: format(new Date(), "yyyy-MM-dd"),
  };
  await fetch("http://localhost:8080/products", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  });
  handleCloseForm();
  getAllProduct();
};

// Hàm cập nhật sản phẩm
const handleShowEdit = (product) => {
  productEdit.value = product;
  showFormEdit.value = true;
};

const handleCloseFormEdit = () => {
  showFormEdit.value = false;
};

const handleUpdate = async () => {
  const updateProduct = {
    id: productEdit.value.id,
    ...productEdit.value,
  };
  await fetch(`http://localhost:8080/products/${productEdit.value.id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(updateProduct),
  });
  handleCloseFormEdit();
  getAllProduct();
};

// Hàm xóa sản phẩm
const handleDelete = async (id) => {
  const confirmDelete = confirm("Bạn có chắc chắn muốn xóa sản phẩm này?");
  if (confirmDelete) {
    await fetch(`http://localhost:8080/products/${id}`, {
      method: "DELETE",
    });
    getAllProduct();
  }
};

// Hàm sắp xếp thời gian
const handleSortTime = async (sortItem) => {
  // Fetch lại danh sách sản phẩm từ API
  const response = await fetch(
    `http://localhost:8080/products?_sort=createdAt&_order=${sortItem}`
  );
  const data = await response.json();

  // Cập nhật lại danh sách sản phẩm sau khi sắp xếp
  products.value = data;
};
const date = "2024-06-12";

console.log(1111, date.split("-").reverse().join("/"));
</script>
<style>
main {
  flex-grow: 1;
  padding: 20px;
}

.header-main {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.btn-add {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

table {
  width: 100%;
  border-collapse: collapse;
  background-color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

table thead {
  background-color: #f1f1f1;
}

table th,
table td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

table img {
  width: 60px;
  height: 60px;
  object-fit: cover;
  max-width: 100%;
}

.btn-edit {
  background-color: #f0ad4e;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-delete {
  background-color: #d9534f;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 5px;
}
</style>
