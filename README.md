# DAWA Autocomplete - Vue2
 

## [](https://www.npmjs.com/package/@psydoc/dawa-autocomplete-vue2#installation)Installation

`npm i @psydoc/dawa-autocomplete-vue2`

Then you need to import and register it:

`import  DawaAutocomplete  from  '@psydoc/dawa-autocomplete-vue2`

`Vue.component('dawa-autocomplete', DawaAutocomplete)`

## [](https://www.npmjs.com/package/@psydoc/dawa-autocomplete-vue2#usage)Usage
<input-tag  v-model="tags"></input-tag>
```
<dawa-autocomplete 
	@autocomplete="yourMethod"
></dawa-autocomplete>
```

## [](https://www.npmjs.com/package/@psydoc/dawa-autocomplete-vue2#events)Events

Name | Type | Description
--- | --- | ---
autocomplete | Function | Returns `city`, `secondaryCity`, `completeAddress`, `streetname` and `streetnumber`