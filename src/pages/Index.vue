<template>
  <q-page>
    <div class="row">
      <div class="q-pa-md col-4">
        <div>POLA WYBORU</div>
        <div class="q-pa-xs">
          <q-select @focus.self="getCitiesList()" multiple filled v-model="city" :options="cities" label="Wybierz Miasto"></q-select>
        </div>
        <p></p>
        <div class="q-pa-xs">
          <q-select @focus.self="getSpecializationList()" multiple filled v-model="specialization" :options="specializations" label="Wybierz Specjalizację"></q-select>
        </div>
        <p></p>
      <div class="q-pa-xs">
        <q-btn @click="city = [],specialization = [],patients = [], visit = '' " label="Reset" color="secondary"></q-btn>
        <q-btn @click="getPatients(),getVisit()" label="Wyszukaj" color="secondary"></q-btn>
      </div>
      </div>
      <div class="q-pa-md col-3">
        <div>PACJENCI</div>
        <q-virtual-scroll :items="patients" style="height: 600px;">
          <template v-slot="{ item, index }">
            <q-field class="col" color="black" label="Imię --- Nazwisko --- Ilość Wizyt" standout="bg-accent text-black" stack-label>
              <div>
                <div :key="index" dense class="self-center col full-width no-outline row text-black">{{item.firstName}} {{item.lastName}} {{item.countVisits}}</div>
              </div>
           </q-field>
          </template>
        </q-virtual-scroll>
      </div>
      <div class="q-pa-md col-5">
        <div>OGÓLNA ILOŚĆ WIZYT</div>
        <div class="col">
            <q-field class="col" color="black" label="Rodzaj Wizyty" standout="bg-accent text-black" stack-label>
              <div>
                <div dense class="self-center col full-width no-outline row text-black">
                  <div class="row">{{visit.visit}}</div>
                </div>
              </div>
           </q-field>
           <p></p>
           <div class="row">
            <q-field class="col" color="black" label="Ilość Wizyt Pacjentów" standout="bg-accent text-black" stack-label>
              <div>
                <div dense class="self-center col full-width no-outline row text-black">
                  <div class="row">{{visit.patientsNumberOfVists}}</div>
                </div>
              </div>
           </q-field>
            <q-field class="col" color="black" label="Ilość Wizyt Ogólnie" standout="bg-accent text-black" stack-label>
              <div>
                <div dense class="self-center col full-width no-outline row text-black">
                  <div class="row">{{visit.numberOfVists}}</div>
                </div>
              </div>
           </q-field>
           </div>
           </div>
      </div>
    </div>
  </q-page>
</template>

<script>
const stringOptions = []
import App from 'src/App.vue'
export default {
  name: 'PageIndex',
  data () {
    return {
      cities: ['ALL'],
      specializations: ['ALL'],
      patients: ['a'],
      visit: "",
      city: [],
      specialization: [],
      local: App.host
    }
  },
  created () {

  },
  methods: {
    getCitiesList () {
      fetch('http://' + this.local + '/info/cities', {
        method: 'GET'
      }).then(response => {
        response.json().then(response => {
          this.cities = response
        })
      })
    },
    getSpecializationList () {
      fetch('http://' + this.local + '/info/specializations', {
        method: 'GET'
      }).then(response => {
        response.json().then(response => {
          this.specializations = response
        })
      })
    },
    getPatients () {
      fetch('http://' + this.local + '/patients/getByCitiesOrSpecialities?cities=' + this.city + '&specialities=' + this.specialization, {
        method: 'GET'
      }).then(response => response.json())
        .then(response => {
          this.patients = response
        })
    },
    getVisit () {
      fetch('http://' + this.local + '/visit/getVisitsBySpecialization?specialization=' + this.specialization + '&cities=' + this.city, {
        method: 'GET'
      }).then(response => response.json())
        .then(response => {
          this.visit = response
        })
    },
    filterFn (val, update) {
      if (val === '') {
        update(() => {
          const needle = val.toLowerCase()
          this.options = this.filters.filter(v => v.toLowerCase().indexOf(needle) > -1)
        })
        return
      }

      update(() => {
        const needle = val.toLowerCase()
        this.options = this.filters.filter(v => v.toLowerCase().indexOf(needle) > -1)
      })
    },
  }
}
</script>
