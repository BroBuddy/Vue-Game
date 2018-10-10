<template>
  <div class="container">
    <h1>Space Marine - Level {{ level }}</h1>
    <hr />

    <div class="row">
      <div class="col-sm-12 col-md-6 col-lg-4">
        <div class="panel panel-info">
          <div class="panel-heading">
            Power
          </div>

          <div class="panel-body">
            <div class="progress">
              <div class="progress-bar progress-bar-info" role="progressbar"
              aria-valuenow="80" aria-valuemin="0" aria-valuemax="100"
              :style="{ width: getPercentage(power, maxPower()) + '%' }">
                  {{ Math.floor(getPercentage(power, maxPower())) + '%' }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="col-sm-12 col-md-6 col-lg-4">
        <div class="panel panel-success">
          <div class="panel-heading">
            Load
          </div>

          <div class="panel-body">
            <div class="progress">
              <div class="progress-bar progress-bar-success" role="progressbar"
              aria-valuenow="80" aria-valuemin="0" aria-valuemax="100"
              :style="{ width: getPercentage(load, maxLoad()) + '%' }">
                  {{ Math.floor(getPercentage(load, maxLoad())) + '%' }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="col-sm-12 col-md-6 col-lg-4">
        <div class="panel panel-danger">
          <div class="panel-heading">
            Cash
          </div>

          <div class="panel-body">
            {{ cash }}$
          </div>
        </div>
        </div>
    </div>
    <hr />

    <div class="row">
      <inventory-character :character="character" :load="load" :maxLoad="maxLoad"
      @addItem="addItem($event)"
      @removeItem="removeItem($event)"></inventory-character>
      <inventory-backpack :backpack="backpack" :character="character" :level="level"
      @addItem="addItem($event)"
      @removeItem="removeItem($event)"></inventory-backpack>
      <inventory-merchant :merchant="merchant" :cash="cash" :load="load" :maxLoad="maxLoad"
      @addItem="addItem($event)"
      @removeItem="removeItem($event)"></inventory-merchant>
    </div>
  </div>
</template>

<script>
import Character from "./components/Character.vue";
import Backpack from "./components/Backpack.vue";
import Merchant from "./components/Merchant.vue";

export default {
  data () {
    return {
      level: 7,
      cash: 4000,
      power: 0,
      load: 0,
      character: [],
      backpack: [],
      merchant: [
        { type: 1, title: 'Power Axe', level: 7, power: 700, price: 490, weight: 14 },
        { type: 1, title: 'Chainsword', level: 5, power: 500, price: 250, weight: 10 },
        { type: 1, title: 'Thunder Hammer', level: 9, power: 900, price: 810, weight: 18 },
        { type: 2, title: 'Bolter', level: 7, power: 700, price: 490, weight: 14 },
        { type: 2, title: 'Bolt Pistol', level: 5, power: 500, price: 250, weight: 10 },
        { type: 2, title: 'Plasma Gun', level: 9, power: 900, price: 810, weight: 18 },
        { type: 3, title: 'Terminator Armour', level: 7, power: 700, price: 490, weight: 14 },
        { type: 3, title: 'Power Armour', level: 5, power: 500, price: 250, weight: 10 },
        { type: 3, title: 'Artificer Armour', level: 9, power: 900, price: 810, weight: 18 },
        { type: 4, title: 'Melta Bombs', level: 7, power: 700, price: 490, weight: 14 },
        { type: 4, title: 'Frag Grenades', level: 5, power: 500, price: 250, weight: 10 },
        { type: 4, title: 'Locator Beacon', level: 9, power: 900, price: 810, weight: 18 },
        { type: 5, title: 'Iron Halo', level: 7, power: 700, price: 490, weight: 14 },
        { type: 5, title: 'Purity Seal', level: 5, power: 500, price: 250, weight: 10 },
        { type: 5, title: 'Battle Standard', level: 9, power: 900, price: 810, weight: 18 },
        { type: 6, title: 'Terminator Honours', level: 7, power: 700, price: 490, weight: 14 },
        { type: 6, title: 'Jump Pack', level: 5, power: 500, price: 250, weight: 10 },
        { type: 6, title: 'Teleport Pack', level: 9, power: 900, price: 810, weight: 18 }
      ]
    }
  },
  methods: {
    maxPower() {
      return this.level * 6 * 100;
    },
    maxLoad() {
      return this.level * 15;
    },
    getPercentage(count, max) {
        return count * 100 / max;
    },
    addItem(event) {
      if (event.newLocation === 'Character') {
        this.character.push(event.item);
        this.power += event.item.power;
      } else if (event.newLocation === 'Backpack') {
        this.backpack.push(event.item);
        this.load += event.item.weight;
      } else if (event.newLocation === 'Merchant') {
        this.merchant.push(event.item);
        this.cash += event.item.price;
      }
    },
    removeItem(event) {
      if (event.oldLocation === 'Character') {
        this.character.splice(event.index, 1);
        this.power -= event.item.power;
      } else if (event.oldLocation === 'Backpack') {
        this.backpack.splice(event.index, 1);
        this.load -= event.item.weight;
      } else if (event.oldLocation === 'Merchant') {
        this.merchant.splice(event.index, 1);
        this.cash -= event.item.price;
      }
    }
  },
  components: {
    'inventory-character': Character,
    'inventory-backpack': Backpack,
    'inventory-merchant': Merchant
  }
}
    
$(document).ready(function() {
    $('[data-toggle="tooltip"]').tooltip();
});
</script>

<style lang="scss" scoped>

</style>
