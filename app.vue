<template>
  <div class="card" :class="{ 'centering': onSubmit }">
    <form v-if="!onSubmit" @submit.prevent="submitForm">
      <label for="email">Email:</label>
      <input type="email" id="email" v-model="email" required>

      <label for="message">Pesan:</label>
      <textarea id="message" v-model="message" required></textarea>
      <button type="submit">Kirim</button>
    </form>
    
    <img v-else src="/loading.gif" alt="loading" width="100">
  </div>
</template>

<script setup>
import axios from 'axios';
import Swal from 'sweetalert2';

const url = "https://script.google.com/macros/s/AKfycbyCSUjXOfXCcuzh8vmg7yzfEn_HJXCpMBdkRgNCf3V-SXAXof60byWIi2Spamst63dV/exec";
let onSubmit = ref(false);
let email = ref('');
let message = ref('');

const submitForm = async () => {
  onSubmit.value = true
  try {
    let data = JSON.stringify({
      email: email.value,
      message: message.value,
    });

    // Kirim data ke Google Spreadsheets menggunakan axios
    await axios.post(url, data,
    {
      headers: {
        'Content-Type': 'text/plain;charset=utf-8',
      },
    });

    Swal.fire({
      icon: 'info',
      title: 'Berhasil Mengirim Data Ke Google Spreadsheet',
      timer: 1000,
    });
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: 'Gagal Mengirim Data Ke Google Spreadsheet',
      timer: 1000,
    });
  }
  onSubmit.value = false
  email.value = '';
  message.value = '';
}
</script>

<style>
 * {
  margin: 0;
  padding: 0;
 }

  #__nuxt {
    font-family: 'Arial', sans-serif;
    background-color: #f5f5f5;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    width: 100vw;
  }

  .card {
    max-width: 400px;
    width: 100%;
    height: 220px;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  .centering {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  label {
    display: block;
    font-size: 14px;
    margin-bottom: 8px;
    color: #333;
  }

  input,
  textarea {
    width: 100%;
    padding: 10px;
    margin-bottom: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }

  textarea {
    resize: vertical;
  }

  button {
    background-color: #4caf50;
    color: #fff;
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
  }

  button:hover {
    background-color: #45a049;
  }
</style>
