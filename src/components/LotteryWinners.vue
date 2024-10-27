<template>
  <div class="search-winners-section">
    <div class="row justify-content-center">
      <div class="col-12 col-md-10">
        <div class="card search-card">
          <div class="card-body d-flex align-items-center">
            <div class="input-group search-input">
              <!-- Display selected winners as tags -->
              <div class="tags">
                <span
                  v-for="(winner, index) in winners"
                  :key="index"
                  class="badge bg-info text-dark me-1"
                >
                  {{ winner.name }}
                  <span class="tag-close" @click="removeWinner(index)">x</span>
                </span>
              </div>
              <input
                type="text"
                class="form-control"
                placeholder="Winners"
                disabled
              />
            </div>
            <button class="btn btn-info ms-2" @click="selectRandomWinner">
              New winner
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

interface Participant {
  name: string;
  birth: string;
  email: string;
  phone: string;
}

export default defineComponent({
  props: {
    participants: {
      type: Array as PropType<Participant[]>,
      required: true,
    },
  },
  data() {
    return {
      winners: [] as Participant[], // State to hold selected winners
    };
  },
  methods: {
    selectRandomWinner() {
      // Ensure there are participants to select from
      if (this.participants.length > 0) {
        // Randomly select a winner from the participants
        const randomIndex = Math.floor(
          Math.random() * this.participants.length
        );
        const selectedWinner = this.participants[randomIndex];

        // Check if the winner is already in the list, if not, add
        if (!this.winners.includes(selectedWinner)) {
          this.winners.push(selectedWinner);
        }
      }
    },
    removeWinner(index: number) {
      // Remove a winner from the list when 'x' is clicked
      this.winners.splice(index, 1);
    },
  },
});
</script>

<style scoped>
.search-winners-section {
  margin-top: 20px;
}

.search-card {
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: white;
  padding: 10px;
}

.search-input {
  width: 100%;
}

.tags {
  display: flex;
  gap: 5px;
  margin-right: 10px;
}

.tag-close {
  cursor: pointer;
  padding-left: 5px;
}

.form-control {
  border-radius: 5px;
}

.btn-info {
  background-color: #00c0e4;
  border-color: #00c0e4;
  color: white;
  border-radius: 5px;
  padding: 10px 15px;
}

.btn-info:hover {
  background-color: #00a4bf;
  border-color: #00a4bf;
}
</style>
