// home.vue
<template>
  <div>
    <button @click="redirectToGoogle">Login with Google</button>
  </div>
</template>

<script>
export default {
  methods: {
    redirectToGoogle() {
      const clientId =
        '952226683996-giormhm5n1ch6vig4n2h0ng5vc9t0p0h.apps.googleusercontent.com';
      const redirectUri = 'http://localhost:9000/';
      const scope = 'https://www.googleapis.com/auth/userinfo.profile';
      const responseType = 'code';
      const url = `https://accounts.google.com/o/oauth2/v2/auth?client_id=${clientId}&redirect_uri=${redirectUri}&scope=${scope}&response_type=${responseType}`;
      window.location.href = url;
    },

    async exchangeCodeForToken(code) {
      try {
        const response = await fetch('http://localhost:9000/token', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ code }),
        });

        if (!response.ok) {
          throw new Error('Network response was not ok');
        }

        const data = await response.json();
        console.log(data);
      } catch (error) {
        console.error('Error:', error);
      }
    },

    created() {
      window.addEventListener('message', event => {
        if (event.data && event.data.code) {
          this.exchangeCodeForToken(event.data.code);
        }
      });
    },
  },
};
</script>
