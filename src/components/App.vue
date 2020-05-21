<template>
  <div id="app">
    <button class="add-button" id="add-button">Add to home screen</button>
    <div class="container">
      <a href="https://duckdev.com" target="_blank">
        <img class="logo" src="../assets/whatsapp.png" />
      </a>
      <form>
        <h1>Contactless WhatsApp</h1>
        <p
          class="form-description"
        >Enter a full 10 digit phone number without international prefix. Omit any zeroes, brackets, or dashes</p>
        <div class="form-group">
          <select v-model="country">
            <option
              v-for="country in countries"
              :key="country.code"
              :value="country.dial_code"
            >{{ country.name }}</option>
          </select>
          <label for="select" class="control-label">Country</label>
          <i class="bar"></i>
        </div>
        <div :class="numberGroupClass">
          <input type="text" required="required" v-model="number" />
          <label for="input" class="control-label">Phone Number</label>
          <i class="bar"></i>
          <span class="error-message" v-if="error">Invalid phone number</span>
        </div>
      </form>
      <div class="button-container">
        <button type="button" class="button" @click="openWhatsApp">
          <span>Open WhatsApp</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import countries from './../data/countries.json';

export default {
  name: 'App',

  data() {
    return {
      number: '',
      error: false,
      country: 91,
      countries: countries
    };
  },

  mounted() {
    let deferredPrompt;
    const addBtn = document.getElementById('add-button');
    addBtn.style.display = 'none';

    window.addEventListener('beforeinstallprompt', e => {
      // Prevent Chrome 67 and earlier from automatically showing the prompt
      e.preventDefault();
      // Stash the event so it can be triggered later.
      deferredPrompt = e;
      // Update UI to notify the user they can add to home screen
      addBtn.style.display = 'block';

      addBtn.addEventListener('click', () => {
        // hide our user interface that shows our A2HS button
        addBtn.style.display = 'none';
        // Show the prompt
        deferredPrompt.prompt();
        // Wait for the user to respond to the prompt
        deferredPrompt.userChoice.then(choiceResult => {
          if (choiceResult.outcome === 'accepted') {
            console.log('User accepted the A2HS prompt');
          } else {
            console.log('User dismissed the A2HS prompt');
          }
          deferredPrompt = null;
        });
      });
    });
  },

  watch: {
    /**
     * When the phone number is changed.
     *
     * Remove the error flag.
     *
     * @since 1.0.0
     */
    number() {
      this.error = false;
    }
  },

  computed: {
    /**
     * Get the class for the form group div.
     *
     * Based on the conditions:
     * - Validation error - `has-error`
     * - Default - `form-group`
     *
     * @since 1.0.0
     *
     * @returns {object}
     */
    numberGroupClass() {
      return {
        'form-group': true,
        'has-error': this.error
      };
    }
  },

  methods: {
    /**
     * Process the form submit.
     *
     * If the number is valid, open it in WhatsApp.
     *
     * @since 1.0.0
     */
    openWhatsApp() {
      // If valid.
      if (this.validate()) {
        this.error = false;
        // Redirect to whatsapp.
        window.location.href = 'https://wa.me/' + this.country + this.number;
      } else {
        // Or set error.
        this.error = true;
      }
    },

    /**
     * Reset the error flag.
     *
     * @since 1.0.0
     */
    resetError() {
      this.error = false;
    },

    /**
     * Validate the current form state.
     *
     * @since 1.0.0
     *
     * @returns {boolean}
     */
    validate() {
      return this.country > 0 && this.number.match(/^\d{10}$/);
    }
  }
};
</script>

<style lang="scss">
@import './style.scss';
</style>
