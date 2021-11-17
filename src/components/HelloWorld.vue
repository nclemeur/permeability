<script setup>
import  { unit } from 'mathjs'
import { reactive, ref, computed } from 'vue'

const a = unit(1990, 'cc mil / (m2  day atm)')
const b = a.toNumber('cc  mm / (m2  day atm)')
const c = a.toNumber('cc  cm / (cm2  s Pa)')

const volumeUnits = ['m3', 'cc'];
const lengthUnits = ['mm', 'mil'];
const areaUnits = ['m2','cm2', 'mm2'];
const timeUnits = ['day', 'second'];
const pressureUnits = ['atm', 'bar', 'kPa', 'Pa', 'mmHg'];

const fromUnit = reactive({ volume: 'cc', length: 'mm', area: 'm2', time: 'day', pressure: 'atm' });
const fromUnitDisplay = computed(()=>{
  const srcUnitStr = `${fromUnit.volume} ${fromUnit.length} / ( ${fromUnit.area} ${fromUnit.time} ${fromUnit.pressure})`
  return srcUnitStr;

});
const fromValue = ref(1);
const toUnit = reactive({ volume: 'cc', length: 'mm', area: 'm2', time: 'day', pressure: 'atm' });
const toUnitDisplay = computed(()=>{
  const dstUnitStr = `${toUnit.volume} ${toUnit.length} / ( ${toUnit.area} ${toUnit.time} ${toUnit.pressure})`
  return dstUnitStr;

});
const toSIDisplay = computed(()=>{
 const from = unit(fromValue.value, toUnitDisplay.value);
 return from.toSI();
});

const toValue = computed( () => {
  const from = unit(fromValue.value, fromUnitDisplay.value);
  return from.toNumber(toUnitDisplay.value);
});
</script>

<template>
  <h1>FROM</h1>
  <div>
  <select v-model="fromUnit.volume">
    <option v-for="item in volumeUnits">{{item}}</option>
  </select>
  <select v-model="fromUnit.length">
    <option v-for="item in lengthUnits">{{item}}</option>
  </select>
  <select v-model="fromUnit.area">
    <option v-for="item in areaUnits">{{item}}</option>
  </select>
  <select v-model="fromUnit.time">
    <option v-for="item in timeUnits">{{item}}</option>
  </select>
  <select v-model="fromUnit.pressure">
    <option v-for="item in pressureUnits">{{item}}</option>
  </select>
  Current selection: {{fromUnitDisplay}}
  <br />
  Value: <input style="margin-top: 1em;" v-model.number="fromValue" type="text" />
  <b style="padding-left: 25px;">SI value:</b> {{ toSIDisplay}} 
  </div>
  <h1>TO</h1>
  <div>
  <select v-model="toUnit.volume">
    <option v-for="item in volumeUnits">{{item}}</option>
  </select>
  <select v-model="toUnit.length">
    <option v-for="item in lengthUnits">{{item}}</option>
  </select>
  <select v-model="toUnit.area">
    <option v-for="item in areaUnits">{{item}}</option>
  </select>
  <select v-model="toUnit.time">
    <option v-for="item in timeUnits">{{item}}</option>
  </select>
  <select v-model="toUnit.pressure">
    <option v-for="item in pressureUnits">{{item}}</option>
  </select>
  Current selection: {{toUnitDisplay}}
  <br />
  Value: {{toValue}}
  </div>

  
  
  
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
