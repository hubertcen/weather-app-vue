<template>
  <div>
    <main>
      <div class="bg-gray-400 w-full rounded-3xl mb-8">
        <div
          class="h-full relative flex items-center justify-between p-2 font-light"
        >
          <input
            type="text"
            class="p-1 flex-1 bg-transparent outline-none placeholder:text-white"
            placeholder="Search for a city"
            v-model="query"
            @keypress="fetchWeather"
          />
        </div>
      </div>

      <div class="max-w-2xl mx-auto mt-20" v-if="weather && weather.main">
        <div class="bg-gray-600 rounded-t-xl shadow-lg overflow-hidden">
          <div class="px-6 py-4">
            <div class="flex justify-between items-center">
              <div class="font-semibold text-xl leading-none text-white">
                {{ weather.name }}, {{ weather.sys.country }}
              </div>
            </div>
            <div class="font-semibold text-base leading-none text-gray-200">
              {{ dateBuilder() }}
            </div>
            <div class="font-semibold text-base leading-none text-gray-200">
              {{ Math.round(weather.main.temp) }}°C
            </div>
            <div class="font-semibold text-base leading-none text-gray-200">
              {{ weather.weather[0].main }}
            </div>
          </div>
        </div>

        <div class="bg-gray-800 px-6 py-4">
          <div class="grid grid-cols-2 gap-4">
            <div>
              <p className="text-sm font-semibold text-gray-300">
                Min Temperature
              </p>
              <p className="text-sm font-semibold text-white">
                {{ Math.round(weather.main.temp_min) }}°C
              </p>
            </div>
            <div>
              <p className="text-sm font-semibold text-gray-300">
                Max Temperature
              </p>
              <p className="text-sm font-semibold text-white">
                {{ Math.round(weather.main.temp_max) }}°C
              </p>
            </div>
            <div>
              <p className="text-sm font-semibold text-gray-300">Wind</p>
              <p className="text-sm font-semibold text-white">
                {{ weather.wind.speed }} m/s
              </p>
            </div>
            <div>
              <p className="text-sm font-semibold text-gray-300">Cloudiness</p>
              <p className="text-sm font-semibold text-white">
                {{ weather.clouds.all }} %
              </p>
            </div>

            <div>
              <p className="text-sm font-semibold text-gray-300">Humidity</p>
              <p className="text-sm font-semibold text-white">
                {{ weather.main.humidity }} %
              </p>
            </div>
            <div>
              <p className="text-sm font-semibold text-gray-300">Pressure</p>
              <p className="text-sm font-semibold text-white">
                {{ weather.main.pressure }} hPa
              </p>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "175f50ac9e5c411302cb91bb59fb5df8",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      this.query = "";
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${month} ${date} ${year}`;
    },
  },
};
</script>

<style></style>
