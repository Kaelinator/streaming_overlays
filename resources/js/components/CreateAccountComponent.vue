<template>
  <section>
    <h1>Create Account</h1>
    <form method="POST" @submit="createAccount">
      <label for="name">Name:</label>
      <input type="text" id="name" name="name" v-model="user.name" />

      <label for="email">Email:</label>
      <input type="email" id="email" name="email" v-model="user.email" />

      <label for="password">Password:</label>
      <input type="password" id="password" name="password" v-model="user.password" />

      <label for="password-confirmation">Confirm password:</label>
      <input
        type="password"
        id="password-confirmation"
        name="passwordConfirmation"
        v-model="user.passwordConfirmation"
      />

      <button type="submit">Create Account</button>
    </form>
  </section>
</template>

<script>
export default {
  methods: {
    createAccount(event) {
      event.preventDefault();

      const body = JSON.stringify({
        name: this.user.name,
        email: this.user.email,
        password: this.user.password,
        password_confirmation: this.user.passwordConfirmation,
        _token: this.csrfToken
      })

      fetch("/register", {
        method: "POST",
        body,
        headers: {
          "Content-Type": "application/json"
        }
      })
        .then(response => {
          console.log(response);
          if (response.status < 200 || response.status > 299) {
            const error = new Error(response.status);
            error.response = response;
            throw error;
          }

          this.$router.push("/");
        })
        .catch(error => {
          const messages = {
            419: "you dun wrong",
            500: "we dun wrong"
          };
          const displayMessage = messages[error.message] || "general error";
          console.error(displayMessage);
        });
    }
  },

  data() {
    return {
      user: {
        name: `Kael${Date.now()}`,
        email: `example${Date.now()}@gmail.com`,
        password: "Password",
        passwordConfirmation: "Password"
      },
      csrfToken: document.querySelector('meta[name="csrf-token"]').content
    };
  }
};
</script>


<style scoped>
section {
  display: flex;
  align-items: center;
  flex-direction: column;
}

form {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 1rem;
  width: 50%;
}

h1 {
  margin: 0.5rem;
}

button {
  grid-column: 1 / 3;
  padding: 1rem;
  justify-self: end;
  background-color: #b2cefe;
}

/* form {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
section {
  width: 
} */
</style>
