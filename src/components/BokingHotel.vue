<script setup>
import { ref, computed } from 'vue'
// memasukkan inputan 
const guestName = ref('')
const roomNumber = ref('')
const bookings = ref([])
const showPendingOnly = ref(false)

// menambahkan bokingan
const addBooking = () => {
  if (guestName.value.trim() && roomNumber.value.trim()) {
    bookings.value.push({
      name: guestName.value,
      room: roomNumber.value,
      checkin: false
    })
    guestName.value = ''
    roomNumber.value = ''
  }
}
// menghapus bokingan
const removeBooking = (index) => {
  bookings.value.splice(index, 1)
}

const toggleCheckin = (booking) => {
  booking.checkin = !booking.checkin
}
// filter
const filteredBookings = computed(() =>
  showPendingOnly.value
    ? bookings.value.filter((b) => !b.checkin)
    : bookings.value
)
</script>

<template>
  <div class="app-container">
    <h1 class="title">🏨 Booking Hotel</h1>

    <div class="card glass">
      <input v-model="guestName" placeholder="Nama Tamu" />
      <input v-model="roomNumber" placeholder="Nomor Kamar" />
      <button @click="addBooking">+ Tambah</button>
    </div>
<!-- checkbox -->
    <div class="card glass">
      <label>
        <input type="checkbox" v-model="showPendingOnly" />
        Tampilkan hanya yang belum check-in
      </label>
    </div>

    <div class="list">
      <div
        class="card glass"
        v-for="(booking, index) in filteredBookings"
        :key="index"
        :class="{ done: booking.checkin }"
      >
        <div class="info">
          <input type="checkbox" v-model="booking.checkin" @change="toggleCheckin(booking)" />
          <span>{{ booking.name }} — Kamar {{ booking.room }}</span>
        </div>
        <button @click="removeBooking(index)">Batalkan</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
/* style box */
* {
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

.app-container {
  min-height: 100vh;
  padding: 2rem;
  background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
  color: #fff;
}

.title {
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 2rem;
}

.card {
  padding: 1rem;
  border-radius: 1rem;
  margin-bottom: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.glass {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.15);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
}

input[type='text'],
input[type='number'] {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  padding: 0.6rem 1rem;
  border: none;
  border-radius: 0.6rem;
  width: calc(50% - 0.5rem);
}

input[type='text']::placeholder {
  color: #ddd;
}

button {
  padding: 0.6rem 1.2rem;
  background: #4caf50;
  border: none;
  border-radius: 0.6rem;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s ease;
}

button:hover {
  background: #388e3c;
}

.list {
  margin-top: 1.5rem;
}

.card.done {
  text-decoration: line-through;
  background: rgba(76, 175, 80, 0.2);
  border-left: 5px solid #66bb6a;
  color: #ccc;
}
</style>
