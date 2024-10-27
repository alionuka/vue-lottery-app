<template>
  <div class="bg-light min-vh-100 d-flex align-items-center">
    <div class="container mt-4">
      <div class="row justify-content-center">
        <div class="col-md-8">
          <div class="card mb-4 shadow-sm">
            <div class="card-body">
              <!-- Пошуковий рядок для переможців -->
              <div class="d-flex justify-content-between align-items-center">
                <div
                  class="form-control winners-field d-flex flex-wrap align-items-center"
                >
                  <!-- Теги переможців -->
                  <span
                    v-for="(winner, index) in winners"
                    :key="index"
                    class="badge bg-info text-white"
                  >
                    {{ winner.name }}
                    <span class="tag-close ms-2" @click="removeWinner(index)"
                      >×</span
                    >
                  </span>
                  <input
                    type="text"
                    class="winners-input"
                    placeholder="Winners"
                    disabled
                  />
                </div>
                <!-- Кнопка деактивується, коли немає учасників або є 3 переможці -->
                <button
                  class="btn btn-info ms-2 custom-btn"
                  @click="selectRandomWinner"
                  :disabled="winners.length >= 3 || participants.length === 0"
                >
                  New winner
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="row justify-content-center mt-3">
        <div class="col-md-8">
          <div class="card mb-4 shadow-sm register-card">
            <div class="card-body">
              <h6 class="form-title">REGISTER FORM</h6>
              <p class="form-subtitle">Please fill in all the fields.</p>
              <RegistrationForm @add-participant="addParticipant" />
            </div>
          </div>
        </div>
      </div>

      <div class="row justify-content-center mt-3">
        <div class="col-md-8">
          <!-- Таблиця учасників -->
          <div class="card mb-4 shadow-sm">
            <div class="card-body">
              <ParticipantsTable :participants="participants" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import LotteryWinners from "./components/LotteryWinners.vue";
import RegistrationForm from "./components/RegistrationForm.vue";
import ParticipantsTable from "./components/ParticipantsTable.vue";

export default defineComponent({
  name: "App",
  components: {
    // eslint-disable-next-line vue/no-unused-components
    LotteryWinners,
    RegistrationForm,
    ParticipantsTable,
  },
  data() {
    return {
      participants: [] as Array<{ name: string }>,
      winners: [] as Array<{ name: string }>,
    };
  },
  methods: {
    addParticipant(participant: { name: string }) {
      this.participants.push(participant);
    },
    selectRandomWinner() {
      if (this.participants.length > 0 && this.winners.length < 3) {
        const randomIndex = Math.floor(
          Math.random() * this.participants.length
        );
        const selectedWinner = this.participants[randomIndex];

        if (!this.winners.includes(selectedWinner)) {
          this.winners.push(selectedWinner);
        }
      }
    },
    removeWinner(index: number) {
      this.winners.splice(index, 1);
    },
  },
});
</script>

<style scoped>
/* CSS стилі залишаються без змін */
.winners-field {
  background-color: white;
  width: 80%;
  border-radius: 5px;
  padding: 8px 12px;
}

.form-control {
  border-radius: 5px;
}

.container {
  background-color: #f9f9f9;
  padding: 2rem;
  border-radius: 6px;
}

.card {
  border-radius: 6px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
  background-color: white;
  border: 1px solid #f0f0f0;
}

.register-card {
  padding-bottom: 2rem;
}

.form-title {
  font-weight: bold;
  text-align: left;
}

.form-subtitle {
  color: #a9a9a9;
  text-align: left;
  margin-top: -10px;
}

.winners-input {
  border: none;
  background: transparent;
  flex: 1;
  min-width: 100px;
  color: #495057;
}

.custom-btn {
  background-color: #00c0e4;
  color: white;
  padding: 6px 20px;
  border-radius: 4px;
  min-width: 100px;
  outline: none;
  box-shadow: none;
  border: none;
  -webkit-tap-highlight-color: transparent;
}

.custom-btn:hover,
.custom-btn:focus,
.custom-btn:active {
  background-color: #00a4bf;
  color: white;
  border-radius: 4px;
  outline: none;
  box-shadow: none;
}

.custom-btn:disabled {
  background-color: #00c0e4;
  color: white;
  opacity: 0.5;
  cursor: not-allowed;
}

.save-btn {
  border-radius: 4px;
  background-color: #00c0e4;
  color: white;
  padding: 6px 20px;
  border-radius: 4px;
  width: 100px;
}

.save-btn:hover {
  background-color: #00a4bf;
}

.shadow-sm {
  box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.05) !important;
}

.badge {
  padding: 0.5em 0.75em;
  font-size: 14px;
  display: inline-block;
  margin-right: 5px;
  margin-top: 5px;
  background-color: #00c0e4;
  color: white;
  border-radius: 3px;
}

.tag-close {
  cursor: pointer;
  padding-left: 5px;
}

.btn-info {
  color: white;
}

.btn[disabled] {
  opacity: 0.5;
  pointer-events: none;
}
</style>
