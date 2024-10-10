<template>
  <div>
    <button @click="handleDetail">Get Detail</button>

    <!-- Bài 4 -->
    <button @click="handleDelete">Delete</button>

    <!-- Bài 5 -->
    <button @click="handleAdd">Thêm</button>

    <!-- Bài 6 -->
    <button @click="handleUpdate">Cập nhật</button>
  </div>
</template>
<script setup>
const getAllProduct = async () => {
  const data = await fetch("http://localhost:8080/products");
  const res = await data.json();
  console.log(res);
};

const getProductById = async (id) => {
  const data = await fetch(`http://localhost:8080/products/${id}`);
  const res = await data.json();
  console.log(res);
};
const handleDetail = () => {
  getProductById(2);
};

const deleteProductById = async (id) => {
  await fetch(`http://localhost:8080/products/${id}`, {
    method: "DELETE",
  });
  getAllProduct();
};

const handleDelete = () => {
  deleteProductById(2);
};

const createProduct = async () => {
  const data = {
    name: "Phở Hà Nội",
    image:
      "https://cdn-i.vtcnews.vn/resize/th/upload/2023/04/21/top-19-quan-pho-ha-noi-ngon-nuc-tieng-an-la-ghien-phan-2-05-1639125425-23532263.jpg",
    price: 30000,
    quantity: 40,
  };
  await fetch("http://localhost:8080/products", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  });
  getAllProduct();
};

const handleAdd = () => {
  createProduct();
};

const updateProduct = async (id) => {
  const data = {
    name: "Phở Hà Nội 123",
  };
  await fetch(`http://localhost:8080/products/${id}`, {
    method: "PATCH",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  });
  getAllProduct();
};

const handleUpdate = () => {
  updateProduct(2);
};
</script>
<style lang=""></style>
