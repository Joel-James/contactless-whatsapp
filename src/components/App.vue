<template>
  <div id="app">
    <div class="container">
      <a href="https://duckdev.com" target="_blank">
        <img class="logo" src="../assets/duck.png" />
      </a>
      <form>
        <h1>Contactless WhatsApp</h1>
        <p
          class="form-description"
        >Enter a full phone number in international format. Omit any zeroes, brackets, or dashes</p>
        <div :class="formGroupClass">
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
export default {
  name: 'App',

  data() {
    return {
      number: '',
      error: false
    };
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
    formGroupClass() {
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
        window.location.href = 'https://wa.me/' + this.number;
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
      return this.number.match(/^\d{12}$/);
    }
  }
};
</script>

<style lang="scss">
@import './style.scss';
</style>
