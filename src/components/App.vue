<template>
  <div id="app">
    <div class="container">
      <form>
        <h1>Contactless WhatsApp</h1>
        <div :class="formGroupClass">
          <input type="text" required="required" v-model="number" />
          <label for="input" class="control-label">Phone Number</label>
          <i class="bar"></i>
          <span
            class="form-help"
          >Enter a full phone number in international format. Omit any zeroes, brackets, or dashes</span>
        </div>
      </form>
      <div class="button-container">
        <button type="button" class="button" @click="openWhatsApp">
          <span>Message</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',

  props: {
    msg: String
  },

  data() {
    return {
      number: '',
      error: false
    };
  },

  created() {
    document.title = 'Contactless WhatsApp';
  },

  watch: {
    number() {
      this.error = false;
    }
  },

  computed: {
    formGroupClass() {
      return {
        'form-group': true,
        'has-error': this.error
      };
    }
  },

  methods: {
    openWhatsApp() {
      if (this.validate()) {
        this.error = false;
        window.location.href =
          'https://api.whatsapp.com/send?phone=' + this.number;
      } else {
        this.error = true;
      }
    },

    resetError() {
      this.error = false;
    },

    validate() {
      return this.number.match(/^\d{10}$/);
    }
  }
};
</script>

<style lang="scss">
@import './style.scss';
</style>
