<template>
  <div class="form-page">
    <div class="glow-wrapper">
      <div class="form-box">
        <h2 class="header">Submit Your Details</h2>
        <form @submit.prevent="submitForm" class="submit-form">  
          <input type="text" v-model="Name" placeholder="Name" required /><br>
          <input type="text" v-model="Address" placeholder="Address" required /><br>
          <input type="number" v-model="Contact" placeholder="Contact" required /><br>
          <div class="image-upload">
            <input type="file" @change="onFileChange" accept="image/*" />
          </div>
          <img v-if="ImagePreview" :src="ImagePreview" alt="Selected Image"
              style="object-fit: cover; max-width: 100px; max-height: 120px;" /><br>
          <button class="button" type="submit">Submit</button>
        </form>
      </div>
    </div>
  </div>
</template>



<script setup>


import axios from 'axios'
import { ref } from 'vue'

const emit = defineEmits(['refresh'])

const Name = ref("")
const Address = ref("")
const Contact = ref("")
const Image = ref(null)
const ImagePreview = ref("")

function onFileChange(event) {
  const file = event.target.files[0]
  if (file) {
    Image.value = file
    const reader = new FileReader()
    reader.onload = (e) => {
      ImagePreview.value = e.target.result
    }
    reader.readAsDataURL(file)
  }
}

const submitForm = async () => {
  const form_Data = new FormData()
  form_Data.append("name", Name.value)
  form_Data.append("address", Address.value)
  form_Data.append("contact", Contact.value)
  form_Data.append("image", Image.value)

  if (!Name.value || !Address.value || !Contact.value || !Image.value) {
    alert("Please fill in all fields and select an image.")
    return
  }
  try {
    await axios.post("https://info-form-backend-production.up.railway.app/submit/", form_Data, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    })
    Name.value = ""
    Address.value = ""
    Contact.value = ""
    Image.value = null
    ImagePreview.value = ""
    emit('refresh')
    alert('Data saved successfully!')
  } catch (error) {
    console.error("Error saving data:", error)
    alert("Failed to save data.")
  }
  
}


</script>

<style scoped>
.form-page {
  height: 100vh;
  background: #0d1117;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Outer wrapper with animated border */
.glow-wrapper {
  width:400px;
  padding: 4px;
  border-radius: 20px;
  background: linear-gradient(270deg, #00ffff, #c21a23, #ff0080, #fa9b0c, #00ffff);
  background-size: 600% 600%;
  animation: glowBorder 6s ease infinite;
}

/* Inner content box */
.form-box {
  background: #1f1f2e;
  border-radius: 18px;
  padding: 30px;
  width: 85%;
  max-width: 500px;
  box-shadow: 0 0 30px rgba(0, 255, 255, 0.15);
}

@keyframes glowBorder {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.header {
  text-align: center;
  font-size: 24px;
  color: #ffffff;
  margin-bottom: 20px;
  text-shadow: 0 0 10px #fa0ee6;
}

.submit-form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.submit-form input[type="text"],
.submit-form input[type="number"],
.submit-form input[type="file"] {
  width: 93%;
  padding: 10px;
  border: 1px solid #00ffe7;
  border-radius: 10px;
  background-color: #2a2a3d;
  color: #ffffff;
}

.submit-form input::placeholder {
  color: #c4c1c1;
}

.submit-form input:focus {
  outline: none;
  border-color: #1affc1;
  background-color: #33334f;
}

.image-upload {
  display: flex;
  justify-content: center;
}

.submit-form img {
  margin-top: 10px;
  border-radius: 8px;
  border: 1px solid #6dc3dd;
}

.button {
  background: #00c896;
  color: white;
  border: none;
  padding: 12px;
  font-size: 16px;
  border-radius: 10px;
  cursor: pointer;
  transition: 0.3s;
}

.button:hover {
  background: #00ffc8;
  color: #000;
}
</style>
