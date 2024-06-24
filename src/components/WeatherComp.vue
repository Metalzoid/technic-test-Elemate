<script lang="ts">
import { useFetch } from '../services/fetch.js'
export default {
  data() {
    return {
      inputValue: null,
      data: null
    }
  },

  methods: {
    fetchData(query) {
      const fetch = useFetch(
        `http://api.weatherstack.com/current?access_key=3e57ca397764c5d6a2af5fab0eaaf971&query=${query}`
      )

      this.data = fetch.data
    },

    inputUpdate() {
      this.fetchData(this.$refs.input.value)
    }
  }
}
</script>

<template>
  <div class="container shadow p-3 mb-5 bg-body rounded">
    <h1 class="fs-2 text-center mb-3">Météo</h1>
    <form @submit.prevent>
      <div class="mb-3" @input="inputUpdate">
        <label for="cityInput" class="form-label">City</label>
        <input
          type="text"
          class="form-control"
          id="cityInput"
          placeholder="Enter a City here ..."
          ref="input"
        />
      </div>
    </form>
    <div class="card text-center" v-if="this.data">
      <div class="card-header">City : {{ this.data.location.name }}</div>
      <div class="card-body">
        <ul class="list-group">
          <li class="list-group-item">
            Actual temperature : {{ this.data.current.temperature }}°C
          </li>
          <li class="list-group-item">
            Weather description : {{ this.data.current.weather_descriptions.join(', ') }}
            <img
              :src="this.data.current.weather_icons"
              alt="Icone Météo"
              style="height: 40px"
              v-if="this.data.current.weather_icons"
            />
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
