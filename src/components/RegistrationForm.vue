<template>
  <div>
    <form @submit.prevent="submitForm">
      <div class="mb-3">
        <label for="name" class="form-label">Name</label>
        <input
          type="text"
          v-model="form.name"
          id="name"
          class="form-control"
          :class="{
            'is-invalid': errors.name,
            'is-valid': !errors.name && form.name,
          }"
          @input="validateField('name')"
          placeholder="Enter user name"
        />
        <div v-if="errors.name" class="invalid-feedback">{{ errors.name }}</div>
      </div>

      <div class="mb-3">
        <label for="birth" class="form-label">Date of Birth</label>
        <input
          type="date"
          v-model="form.birth"
          id="birth"
          class="form-control"
          :class="{
            'is-invalid': errors.birth,
            'is-valid': !errors.birth && form.birth,
          }"
          @input="validateField('birth')"
          placeholder="mm/dd/yyyy"
        />
        <div v-if="errors.birth" class="invalid-feedback">
          {{ errors.birth }}
        </div>
      </div>

      <div class="mb-3">
        <label for="email" class="form-label">Email</label>
        <input
          type="email"
          v-model="form.email"
          id="email"
          class="form-control"
          :class="{
            'is-invalid': errors.email,
            'is-valid': !errors.email && form.email,
          }"
          @input="validateField('email')"
          placeholder="Enter email"
        />
        <div v-if="errors.email" class="invalid-feedback">
          {{ errors.email }}
        </div>
      </div>

      <div class="mb-3">
        <label for="phone" class="form-label">Phone Number</label>
        <input
          type="tel"
          v-model="form.phone"
          id="phone"
          class="form-control"
          :class="{
            'is-invalid': errors.phone,
            'is-valid': !errors.phone && form.phone,
          }"
          @input="validateField('phone')"
          placeholder="Enter Phone number"
        />
        <div v-if="errors.phone" class="invalid-feedback">
          {{ errors.phone }}
        </div>
      </div>

      <div class="d-flex justify-content-end mt-3">
        <button type="submit" class="btn btn-primary custom-btn">Save</button>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      form: {
        name: "",
        birth: "",
        email: "",
        phone: "",
      } as {
        name: string;
        birth: string;
        email: string;
        phone: string;
      },
      errors: {} as Record<string, string>,
    };
  },
  mounted() {
    // Викликає календар при кліку на полі дати
    const dateInput = document.getElementById("birth") as HTMLInputElement & {
      showPicker?: () => void;
    };
    if (dateInput && typeof dateInput.showPicker === "function") {
      dateInput.addEventListener("click", () => {
        if (typeof dateInput.showPicker === "function") {
          dateInput.showPicker(); // Викликаємо showPicker після перевірки
        }
      });
    }
  },

  methods: {
    validateField(field: string) {
      this.errors[field] = "";

      if (field === "name" && !this.form.name) {
        this.errors.name = "Name is required";
      }

      if (field === "birth") {
        if (!this.form.birth) {
          this.errors.birth = "Date of birth is required";
        } else if (new Date(this.form.birth) > new Date()) {
          this.errors.birth = "Date of birth cannot be in the future";
        }
      }

      if (field === "email") {
        if (!this.form.email || !this.validEmail(this.form.email)) {
          this.errors.email = "Invalid email";
        }
      }

      if (
        field === "phone" &&
        (!this.form.phone || !this.validPhone(this.form.phone))
      ) {
        this.errors.phone = "Invalid phone number";
      }
    },
    submitForm() {
      this.validateField("name");
      this.validateField("birth");
      this.validateField("email");
      this.validateField("phone");

      if (Object.keys(this.errors).every((key) => !this.errors[key])) {
        this.$emit("add-participant", { ...this.form });
        this.form.name = "";
        this.form.birth = "";
        this.form.email = "";
        this.form.phone = "";
        this.errors = {}; // Очищаємо помилки після відправки
      }
    },
    validEmail(email: string) {
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return re.test(email);
    },
    validPhone(phone: string) {
      const re = /^\+?\d{10,14}$/;
      return re.test(phone);
    },
  },
});
</script>

<style scoped>
.form-control {
  background-color: #f5f5f5; /* Світло-сірий фон для полів вводу */
  color: #495057; /* Колір тексту */
  border: 1px solid #e9ecef; /* Світло-сіра границя */
  border-radius: 5px;
  padding: 8px 12px; /* Внутрішні відступи */
}

.form-control:focus {
  background-color: #f5f5f5 !important; /* Світло-сірий фон для полів при фокусі */
  outline: none;
  box-shadow: none;
}

.form-control::placeholder {
  color: #adb5bd; /* Світло-сірий колір для тексту-підказки */
}

input[type="date"]::-webkit-calendar-picker-indicator {
  display: none;
}

input:-webkit-autofill {
  background-color: #f5f5f5 !important;
  -webkit-box-shadow: 0 0 0px 1000px #f5f5f5 inset;
  color: #495057;
}

input[type="date"] {
  color: #495057;
}

input[type="date"]::-webkit-datetime-edit-text,
input[type="date"]::-webkit-datetime-edit-month-field,
input[type="date"]::-webkit-datetime-edit-day-field,
input[type="date"]::-webkit-datetime-edit-year-field {
  color: #adb5bd;
}

.is-invalid {
  border-color: #dc3545;
}

.is-valid {
  border-color: #28a745;
}

.form-label {
  font-weight: bold;
}

.custom-btn {
  border-radius: 4px;
  background-color: #00c0e4;
  color: white;
  padding: 6px 20px;
  min-width: 100px;
  outline: none;
  box-shadow: none;
  border: none;
  -webkit-tap-highlight-color: transparent;
}

.custom-btn:focus,
.custom-btn:active {
  outline: none;
  box-shadow: none;
}

.custom-btn:hover {
  background-color: #00a4bf;
}

.custom-btn:disabled {
  background-color: #bdbdbd;
  cursor: not-allowed;
}

.mt-3 {
  margin-top: 1.5rem;
}
</style>
