<template>
  <div class="modal-overlay" @click.self="closeModal">
    <div class="modal-window" @click.stop>
      <div class="date-selector">
        <button @click="changeDate(-1)">← 이전 날</button>
        <input type="date" v-model="selectedDate" />
        <button @click="changeDate(1)">다음 날 →</button>
      </div>
      <h2>{{ selectedRoom.roomName }} 예약 정보</h2>
      <ul>
        <li v-for="reservation in filteredReservations" :key="reservation.eventIdx">
          {{ reservation.startedAt }}부터 {{ reservation.closedAt }}까지
        </li>
      </ul>
      <button class="close-button" @click="$emit('close')">닫기</button>
    </div>
  </div>
</template>

<script>
import { useMeetingRoomStore } from '@/stores/useMeetingRoomStore';

export default {
  data() {
    return {
      selectedDate: '',
    };
  },
  created() {
    this.resetDateToToday();
  },
  methods: {
    resetDateToToday() {
      const today = new Date();
      const formattedDate = today.toISOString().split('T')[0];
      this.selectedDate = formattedDate;
    },
    changeDate(days) {
      const currentDate = new Date(this.selectedDate);
      currentDate.setDate(currentDate.getDate() + days);
      const formattedDate = currentDate.toISOString().split('T')[0];
      this.selectedDate = formattedDate;
    },
    closeModal() {
      this.$emit('close');
    },
  },
  computed: {
    selectedRoom() {
      return useMeetingRoomStore().selectedRoom;
    },
    filteredReservations() {
      if (!this.selectedDate) return [];
      return this.selectedRoom.reservations.filter(reservation => {
        const reservationDate = reservation.startedAt.split(' ')[0];
        return reservationDate === this.selectedDate;
      });
    },
  },
};
</script>

<style>
/* 모달 오버레이 스타일 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* 반투명 검정색 배경 */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000; /* 다른 요소 위에 오도록 함 */
}

/* 모달 윈도우 스타일 */
.modal-window {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  max-width: 500px; /* 모달의 최대 너비 */
  z-index: 1001; /* 모달 오버레이보다 더 위에 오도록 함 */
}

/* 닫기 버튼 스타일 */
.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  border: none;
  background-color: transparent;
  cursor: pointer;
}

.date-selector {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 1em; /* 여백 조정 */
}

.date-selector button {
  border: none;
  background-color: #f0f0f0;
  padding: 0.5em 1em;
  cursor: pointer;
  transition: background-color 0.3s;
}

.date-selector button:hover {
  background-color: #e0e0e0;
}

.date-selector input[type="date"] {
  margin: 0 1em;
}
</style>

