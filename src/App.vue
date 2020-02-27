<template>
  <div id="app">
    <div class="person" v-for="(person, i) in persons" :key="'person_' + i">
      <img :src="person.picture.large">
      <div>
        <p>{{ person.name.first }} {{ person.name.last }}</p>
        <ul>
          <li>
            <strong>Birthday:</strong> {{ formatDate(person.dob.date) }}
          </li>
          <li class="text-capitalize">
            <strong>Location:</strong> {{ person.location.city }},
            {{ person.location.state }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
const GET_USER_URL = 'https://randomuser.me/api/'

export default {
  name: 'App',
  components: {},
  data () {
    return {
      persons: []
    }
  },
  methods: {
    /**
     * @description Gets five random users and assigns them to the variable person
     */
    getUsers () {
      const self = this
      let i = 0
      while (i < 5) {
        fetch(GET_USER_URL)
          .then(response => response.json())
          .then(response => {
            self.persons.push(response.results[0])
          })
        i++
      }
    },
    /**
     * @description Formats a date string to readable form
     * @return {string} date in locale (ru) format, e.g. '27.02.20'
     * @param {string} dateStr - Date as string in simplified extended ISO format (ISO 8601), e.g. "1993-07-20T09:44:18.674Z"
     */
    formatDate (dateStr) {
      return new Date(dateStr).toLocaleString('ru', {
        day: '2-digit',
        month: '2-digit',
        year: '2-digit'
      })
    }
  },
  mounted () {
    /**
     * @description Gets users when the user scrolls to the bottom of the window
     */
    window.onscroll = () => {
      const ALLOWABLE_ERROR_INT = 10
      const IS_BOTTOM_OF_WINDOW = document.documentElement.scrollTop + window.innerHeight + ALLOWABLE_ERROR_INT > document.documentElement.offsetHeight

      if (IS_BOTTOM_OF_WINDOW) {
        this.getUsers()
      }
    }
  },
  beforeMount () {
    this.getUsers()
  }
}
</script>

<style>
  #app {
    font-family: Verdana, Tahoma, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #474747;
  }

  .person {
    background: rgb(240,240,240);
    background: linear-gradient(0deg, rgba(240,240,240,1) 0%, rgba(229,229,229,1) 100%);
    border-radius: 7px;
    width: 270px;
    margin: 0 auto 15px auto;
    padding: 60px 15px 34px;
  }

  .person ul {
    text-align: left;
    font-size: 12px;
  }

  .person ul li {
    list-style: none;
  }

  img {
    border-radius: 100px;
  }

  p:first-child {
    text-transform: capitalize;
    font-size: 20px;
    font-weight: bold;
  }

  .text-capitalize {
    text-transform: capitalize;
  }
</style>
