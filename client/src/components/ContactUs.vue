<template>
  <div class="container">
    <div class="custom-breadcrumns border-bottom">
      <div class="container">
        <a href="/">Home</a>
      </div>
    </div>
    <div class="CTSUP">
      <div id="contact-form" class="contact-form">
        <h1 class="contact-form_title">Contact Form</h1>
        <div class="separator"></div>

        <div v-if="isSending" class="loading">Sendig...</div>

        <form class="form" @submit="onSubmit">
          <input
            required
            name="First name"
            v-model="contact.name"
            placeholder="First Name"
            type="text"
            autocomplete="off"
          />
          <input
            required
            name="Last name"
            v-model="contact.Lname"
            placeholder="Last name"
            type="text"
            autocomplete="off"
          />
          <input
            required
            name="email"
            v-model="contact.email"
            placeholder="E-mail"
            type="email"
            autocomplete="off"
          />
          <textarea
            name="message"
            v-model="contact.message"
            rows="4"
            placeholder="Message"
          ></textarea>
          <button class="button">Send</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    contact: {
      name: "",
      Lname: "",
      email: "",
      message: "",
    },
    isSending: false,
  }),
  methods: {
    /**
     * Clear the form
     */
    clearForm() {
      for (let field in this.contact) {
        this.contact[field] = "";
      }
    },
    /**
     * Handler for submit
     */
    onSubmit(evt) {
      evt.preventDefault();
      this.isSending = true;
      setTimeout(() => {
        // Build for data
        let form = new FormData();
        for (let field in this.contact) {
          form.append(field, this.contact[field]);
        }
        // Send form to server
        axios
          .post("http://localhost:7000/api/contact/ContactUs", this.contact)
          .then((response) => {
            if (response) {
              console.log(response.data);
              this.clearForm();
              this.isSending = false;
            }
          })
          .catch((e) => {
            console.log(e);
          });
      }, 1000);
    },
  },
};
</script>

<style scoped>
body {
  background: #f1f1f1;
  font-family: "Roboto", sans-serif;
}
div#contact-form {
  margin-top: 160px;
}
.contact-form {
  font-family: 16px;
  margin: 0 auto;
  max-width: 600px;
  width: 100%;
}
.contact-form .separator {
  border-bottom: solid 1px #ccc;
  margin-bottom: 15px;
}
.contact-form .form {
  display: flex;
  flex-direction: column;
  font-size: 16px;
}
.contact-form_title {
  color: #333;
  text-align: left;
  font-size: 28px;
}
.contact-form input[type="email"],
.contact-form input[type="text"],
.contact-form textarea {
  border: solid 1px #e8e8e8;
  font-family: "Roboto", sans-serif;
  padding: 10px 7px;
  margin-bottom: 15px;
  outline: none;
}
.contact-form textarea {
  resize: none;
}
.contact-form .button {
  background: #0e3ba0e3;
  border: solid 1px #0d0dfb;
  color: white;
  cursor: pointer;
  padding: 10px 50px;
  text-align: center;
  text-transform: uppercase;
}
.contact-form input[type="email"],
.contact-form input[type="text"],
.contact-form textarea,
.contact-form .button {
  font-size: 15px;
  border-radius: 3px;
}
.CTSUP {
  margin-top: -82px;
}
</style>