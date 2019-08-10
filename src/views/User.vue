<template>
  <div class="user">
    <h1>This is a user page</h1>
  </div>
</template>

<script>
  export default {
    name: 'user',
    data: () => {
      return {
        users: [],
      }
    },
    created: function () {
      const self = this;
      axios.get('https://randomuser.me/api/?results=10')
        .then(function (response) {
          console.log(response);
          self.users = response.data.results;
        })
    }
  }
</script>