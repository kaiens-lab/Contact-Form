<script setup>
import { onMounted, onUnmounted, ref } from "vue";

// Initial State
const valid = ref(false);
const firstName = ref("");
const lastName = ref("");
const emailAddress = ref("");
const selectedRadio = ref(null);
const message = ref("");
const consentCheck = ref(false);
const successMessage = ref(false);

// Error Messages
const firstNameError = ref([]);
const lastNameError = ref([]);
const emailAddressError = ref([]);
const selectedRadioError = ref([]);
const messageError = ref([]);
const consentCheckError = ref([]);

const validForm = () => {
  firstNameError.value = [];
  lastNameError.value = [];
  emailAddressError.value = [];
  selectedRadioError.value = [];
  messageError.value = [];
  consentCheckError.value = [];

  if (!firstName.value) {
    firstNameError.value.push("This Field is required.");
  }
  if (!lastName.value) {
    lastNameError.value.push("This Field is required.");
  }
  if (!emailAddress.value) {
    emailAddressError.value.push("Please enter a valid email address.");
  } else if (!/.+@.+\..+/.test(emailAddress.value)) {
    emailAddressError.value.push("Please enter a valid email address.");
  }
  if (selectedRadio.value === null) {
    selectedRadioError.value.push("Please select a query type.");
  }
  if (!message.value) {
    messageError.value.push("This field is required.");
  }
  if (!consentCheck.value) {
    consentCheckError.value.push(
      "To submit this form, please consent to being contacted."
    );
  }

  if (
    firstNameError.value.length === 0 &&
    lastNameError.value.length === 0 &&
    emailAddressError.value.length === 0 &&
    selectedRadioError.value.length === 0 &&
    messageError.value.length === 0 &&
    consentCheckError.value.length === 0
  ) {
    submitForm();
  }
};

// submitForm

const submitForm = () => {
  successMessage.value = true;

  setTimeout(() => {
    firstName.value = "";
    lastName.value = "";
    emailAddress.value = "";
    selectedRadio.value = null;
    message.value = "";
    consentCheck.value = false;

    firstNameError.value = [];
    lastNameError.value = [];
    emailAddressError.value = [];
    selectedRadioError.value = [];
    messageError.value = [];
    consentCheckError.value = [];

    valid.value = false;
  }, 1000);

  setTimeout(() => {
    successMessage.value = false;
  }, 1500);
};

// MobileVision

const isMobile = ref(window.innerWidth < 600);

const handleMobileVision = () => {
  isMobile.value = window.innerWidth < 600;
};

onMounted(() => {
  window.addEventListener("resize", handleMobileVision);
});

onUnmounted(() => {
  window.removeEventListener("resize", handleMobileVision);
});

</script>

<template>
  <v-container class="container pa-10 mt-16">
    <!-- 成功訊息 -->
    <v-alert
      v-if="successMessage"
      class="success-msg-container"
    >
      <p class="success-content msg-big"><img src="./assets/images/icon-success-check.svg" class="success-img"></img>Message Sent!</p>
      <p class="success-content msg-small">
        Thanks for completing the form.We'll be in touch soon!
      </p>
    </v-alert>
    <h2 class="mb-8 title">Contact Us</h2>
    <v-form
      ref="form"
      v-model="valid"
      @submit.prevent="submitForm"
      style="font-size: 1rem"
    >
      <!-- Name -->
      <v-row >
        <v-col :cols="isMobile ? 12 : 6">
          <v-label class="mb-2"
            >First Name <span class="mark ml-2">*</span></v-label
          >
         
          <v-text-field
            v-model="firstName"
            color="hsl(169, 82%, 27%)"
            base-color="hsl(186, 15%, 59%)"
            variant="outlined"
            density="comfortable"
            required
          ></v-text-field>
          <div v-if="firstNameError.length !== 0" class="err-msg">
            This field is required
          </div>
        </v-col>

        <v-col :cols="isMobile ? 12 : 6">
          <v-label class="mb-2"
            >Last Name <span class="mark ml-2">*</span></v-label
          >
          <v-text-field
            v-model="lastName"
            variant="outlined"
            color="hsl(169, 82%, 27%)"
            base-color="hsl(186, 15%, 59%)"
            density="comfortable"

            required
          ></v-text-field>
          <div v-if="lastNameError.length !== 0" class="err-msg">
            This field is required
          </div>
        </v-col>
      </v-row>

      <!-- Email Address -->
      <v-row class="mb-2 mt-0">
        <v-col :cols="12">
          <v-label class="mb-2"
            >Email Address<span class="mark ml-2">*</span>
          </v-label>
          <v-text-field
            color="hsl(169, 82%, 27%)"
            base-color="hsl(187, 24%, 22%)"
            v-model="emailAddress"
            variant="outlined"
            density="comfortable"
            required
          ></v-text-field>
          <div v-if="emailAddressError.length !== 0" class="err-msg">
            Please enter a valid email address.
          </div>
        </v-col>
      </v-row>

      <!-- Query Type -->
      <v-label class="mb-2"
        >Query Type <span class="mark ml-2">*</span></v-label
      >
      <v-row>
        <v-radio-group
          v-model="selectedRadio"
          class="mb-6"
          inline
          hide-details
          required
        >
          <v-col :cols="isMobile ? 12 : 6">
            <div
              class="radio-area"
              :class="{ 'selected-radio': selectedRadio === 'general' }"
            >
              <div class="radio-button">
                <div
                  class="radio-selected"
                  :style="{
                    display: selectedRadio === 'general' ? 'inline' : 'none',
                  }"
                ></div>
              </div>
              <v-radio
                class="ml-n6"
                value="general"
                label="General Enquiry"
                color="teal-darken-2"
              ></v-radio>
            </div>
          </v-col>

          <v-col :cols="isMobile ? 12 : 6">
            <div
              class="radio-area"
              :class="{ 'selected-radio': selectedRadio === 'suport' }"
            >
              <div class="radio-button">
                <div
                  class="radio-selected"
                  :style="{
                    display: selectedRadio === 'suport' ? 'inline' : 'none',
                  }"
                ></div>
              </div>
              <v-radio
                class="ml-n6"
                value="suport"
                label="Support Request"
                color="teal-darken-2"
              ></v-radio>
            </div>
          </v-col>
        </v-radio-group>
      </v-row>
      <v-row class="ml-0">
        <div v-if="selectedRadioError.length !== 0" class="err-msg">
          Please select a query type.
        </div>
      </v-row>

      <!-- Message -->
      <v-row >
        <v-col :cols="12">
          <v-label class="mb-2"
            >Message<span class="mark ml-2">*</span></v-label
          >
          <v-textarea
            v-model="message"
            variant="outlined"
            color="hsl(169, 82%, 27%)"
            base-color="hsl(187, 24%, 22%)"
            flat
            hide-details
            class="custom-textarea mb-2"
         
          ></v-textarea>
          <div v-if="messageError.length !== 0" class="err-msg mb-6">
            This field is required
          </div>
        </v-col>
      </v-row>

      <!-- Consent/ -->
      <v-row style="margin-top: 0" class="mb-4">
        <v-col cols="1"
          ><v-checkbox v-model="consentCheck" required
            ><div
              class="checked"
              :style="{
                display: consentCheck === true ? 'inline' : 'none',
              }"
            >
              <img
                src="./assets/images/icon-checkbox-check.svg"
              /></div></v-checkbox
        ></v-col>

        <v-col cols="11"
          ><p class="consent-text">
            I consent to being contacted by the team<span class="mark ml-2"
              >*</span
            >
          </p></v-col
        >
        <div v-if="consentCheckError.length !== 0" class="err-msg ml-2">
          To submit this form, please consent to being contacted
        </div>
      </v-row>

      <!-- Submit-button -->
      <v-row>
        <v-col :cols="12">
          <v-btn @click="validForm" class="submit-btn" style="height:3.6875rem">Submit</v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<style>
.title {
  font-size: 2rem;
}

.v-text-field {
  margin: 0;
}

.v-radio-group {
  height: 51px;
}



.radio-area {
  border: 1px solid hsl(186, 15%, 59%, 0.5);
  border-radius: 4px;
  height: 51px;
  display: flex;
  align-items: center;
}

.radio-button {
  border: 1px solid hsl(186, 15%, 59%);
  width: 24px;
  height: 24px;
  border-radius: 50%;
  margin-left: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.radio-selected {
  display: none;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background-color: hsl(169, 82%, 27%);
}

.selected-radio {
  background-color: hsl(148, 38%, 91%);
}

.v-label,
p {
  color: hsl(187, 24%, 22%);
  opacity: 1;
}



.v-checkbox {
  border: 1px solid hsl(169, 82%, 27%);
  height: 1.2rem;
  width: 1.2rem;
  margin-top: 5px;
}

.container {
  width:100%;
  max-width: 736px;
  height:auto;
  background-color: white;
  border-radius: 20px;
}

.mark {
  color: hsl(169, 82%, 27%);
}

.submit-btn {
  background-color: hsl(169, 82%, 27%);
  width: 100%;
  border-radius: 8px;
  color: hsl(0, 0%, 100%);
}


.input-area-selected {
  border: 2px solid hsl(169, 82%, 27%);
}

.checked {
  margin-left: 18px;
  margin-top: -32px;
}

.consent-text {
  margin-left: -20px;
}

.err-msg {
  color: hsl(0, 66%, 54%);
}

.success-msg-container {
  width: 498px;
  height: 107px;
  display: flex;
  justify-content: center;
  padding: 10px;
  margin: 1.5rem;
  background-color: hsl(187, 24%, 22%);
  border-radius: 10px;
  font-weight: bold;
  position: absolute;
  top: 5%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.success-content {
  color: white;
  font-weight: normal;
}

.success-img{
  margin-right:10px;
  margin-bottom:-5px;
}

.msg-big {
  font-size: 1.2rem;
  font-weight: 700;
}

.msg-small {
  font-size: 0.975rem;
}

@media (max-width: 600px) {
  .v-radio-group {
  height: 110px;
}

.consent-text {
  margin-left: 0px;
}

}


</style>
