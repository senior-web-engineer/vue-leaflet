<template>
  <div class="q-pa-md">
    <div class="row" style="margin: 20px">
      <q-card outlined style="width: 80%">
        <q-list v-if="!location.position">
          <q-item>
            <q-item-section avatar>
              <q-btn round color="deep-orange" icon="edit_location" />
            </q-item-section>
          </q-item>
          <q-item>
            <q-item-section>
              <q-item-label>No Location selected</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
        <q-list v-else>
          <q-item>
            <q-item-section avatar>
              <q-btn round color="deep-orange" icon="edit_location" />
            </q-item-section>
            <q-item-section>
              <q-item-label class="title text-wrap">{{
                location.address
              }}</q-item-label>
            </q-item-section>
          </q-item>
          <q-item>
            <q-item-section>
              <q-item-label caption>
                Latitude: {{ location.position.lat }}
              </q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label caption>
                Longitude: {{ location.position.lng }}
              </q-item-label>
            </q-item-section>
          </q-item>
          <q-item>
            <q-btn color="purple" label="Reset" @click="reset" />
          </q-item>
        </q-list>
      </q-card>
    </div>
    <div class="row">
      <div class="col-4">
        <q-select
          v-model="select"
          :hint="`${select.state}, ${select.abbr}`"
          :options="items"
          option-label="state"
          @input="
            (val) => {
              change(val);
            }
          "
          item-text="state"
          item-value="abbr"
          label="Select"
        />
      </div>
      <div class="col-8">
        <q-card class="my-card">
          <q-card-section>
            <GeolocationSelector
              v-model="location"
              :key="key"
              :selectedlocation="selectedlocation"
              style="height: 400px"
            />
          </q-card-section>
        </q-card>
      </div>
    </div>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import "leaflet-geosearch/dist/geosearch.css";
import GeolocationSelector from "components/GeolocationSelector.vue";

const defaultLocation = {
  state: "Florida",
  abbr: "FL",
  position: {
    lat: 27.7567667,
    lng: -81.4639835,
  },
};
export default {
  name: "PageIndex",
  components: {
    GeolocationSelector,
  },

  data: () => ({
    dialog: false,
    location: defaultLocation,
    selectedlocation: defaultLocation,
    key: 1,
    select: defaultLocation,
    items: [
      {
        state: "Florida",
        abbr: "FL",
        position: { lat: 27.7567667, lng: -81.4639835 },
      },
      {
        state: "South Carolina",
        abbr: "SC",
        position: {
          lat: 33.6874388,
          lng: -80.4363743,
        },
      },
      {
        state: "North Carolina",
        abbr: "NC",
        position: {
          lat: 35.6729639,
          lng: -79.0392919,
        },
      },
    ],
  }),
  methods: {
    reset() {
      this.key += 1;
      this.location = this.selectedlocation = this.select = defaultLocation;
    },
    change(val) {
      console.log(val);
      this.selectedlocation = val;
    },
  },
};
</script>
