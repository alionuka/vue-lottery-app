<template>
  <div class="card p-4 mb-4">
    <h5 class="card-title">REGISTER FORM</h5>
    <p class="text-muted">Please fill in all the fields.</p>
    <form @submit.prevent="onSubmit" @keyup.enter="onSubmit">
      <div class="form-group mb-3">
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

      <div class="form-group mb-3">
        <label for="dob" class="form-label">Date of Birth</label>
        <input
          type="date"
          v-model="form.dob"
          id="dob"
          class="form-control"
          :class="{
            'is-invalid': errors.dob,
            'is-valid': !errors.dob && form.dob,
          }"
          @input="validateField('dob')"
          placeholder="Enter date of birth"
        />
        <div v-if="errors.dob" class="invalid-feedback">{{ errors.dob }}</div>
      </div>

      <div class="form-group mb-3">
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

      <div class="form-group mb-3">
        <label for="phone" class="form-label">Phone number</label>
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
          placeholder="Enter phone number"
        />
        <div v-if="errors.phone" class="invalid-feedback">
          {{ errors.phone }}
        </div>
      </div>

      <div class="text-end mt-3">
        <button type="submit" class="btn btn-primary">Save</button>
      </div>
    </form>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  props: {
    initialData: {
      type: Object,
      default: () => ({}),
    },
  },
  setup(props, { emit }) {
    const form = ref({
      name: props.initialData.name || "",
      dob: props.initialData.dob || "",
      email: props.initialData.email || "",
      phone: props.initialData.phone || "",
    });
    const errors = ref({});

    const validateEmail = (email) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    const validatePhone = (phone) => /^\+?[0-9]{10,15}$/.test(phone);

    const validateField = (field) => {
      errors.value[field] = "";

      if (field === "name" && !form.value.name) {
        errors.value.name = "Name is required.";
      }

      if (field === "dob") {
        if (!form.value.dob) {
          errors.value.dob = "Date of Birth is required.";
        } else if (new Date(form.value.dob) > new Date()) {
          errors.value.dob = "Date of Birth cannot be in the future.";
        }
      }

      if (field === "email") {
        if (!form.value.email || !validateEmail(form.value.email)) {
          errors.value.email = "Invalid email format.";
        }
      }

      if (
        field === "phone" &&
        (!form.value.phone || !validatePhone(form.value.phone))
      ) {
        errors.value.phone = "Invalid phone number format.";
      }
    };

    const onSubmit = () => {
      validateField("name");
      validateField("dob");
      validateField("email");
      validateField("phone");

      if (Object.keys(errors.value).some((key) => errors.value[key])) {
        return;
      }

      emit("add-winner", { ...form.value });

      form.value.name = "";
      form.value.dob = "";
      form.value.email = "";
      form.value.phone = "";
      errors.value = {};
    };

    return { form, errors, validateField, onSubmit };
  },
};
</script>

<style scoped>
.card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  background-color: #ffffff;
}

.card-title {
  font-size: 1.25rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.text-muted {
  font-size: 0.875rem;
  color: #6c757d;
}

.btn-primary {
  background-color: #007bff;
  border-color: #007bff;
}

.btn-primary:hover {
  background-color: #0056b3;
  border-color: #004085;
}

.is-invalid {
  border-color: #dc3545;
}

.is-valid {
  border-color: #28a745;
}

.invalid-feedback {
  color: #dc3545;
}
</style>
