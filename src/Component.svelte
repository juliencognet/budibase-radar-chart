<script>
  import { getContext } from "svelte"
  import { chart } from "svelte-apexcharts";
  import { merge } from 'lodash';
  
  export let dataProvider //
  export let serieField //
  export let selectionMethod //
  export let categoryField //
  export let valueField //
  export let aggregationMethod //
  export let valueField1 //
  export let labelField1 //
  export let valueField2 //
  export let labelField2 //
  export let valueField3 //
  export let labelField3 //
  export let valueField4 //
  export let labelField4 //
  export let valueField5 //
  export let labelField5 //
  export let valueField6 //
  export let labelField6 //
  export let valueField7 //
  export let labelField7 //
  export let valueField8 //
  export let labelField8 //
  export let valueField9 //
  export let labelField9 //
  export let valueField10 //
  export let labelField10 //
  export let chartHeight //
  export let chartWidth //
  export let chartTitle //
  export let minAxis //
  export let maxAxis //
  export let axisTickAmount //
  export let colorPalette //
  export let displayDataLabels //
  export let advancedChartOptions //
  
  const loading = getContext("loading")
  let canBeDisplayed = true;
  let errorMessages = [];

  // ----------------------------------------------------------------
  // Check configuration of chart
  if (dataProvider==null){
    errorMessages.push("Data provider must be defined.");
  }
  const serieAndCategoryAuthorizedFieldTypesAndMapping = ["number","string","barcodeqr","options"];
  const errorIfNotInAuthorizedTypes = "must be either a number, a string, options or a barcode"
  if (serieField!=null){
    if (!serieAndCategoryAuthorizedFieldTypesAndMapping.includes(dataProvider.schema[serieField].type)){
      errorMessages.push("Serie field "+errorIfNotInAuthorizedTypes);
    }
  }

  if (selectionMethod == "category_value_fields"){
    if (categoryField==null) {
      errorMessages.push("Category Field must be defined");
    } else if (!serieAndCategoryAuthorizedFieldTypesAndMapping.includes(dataProvider.schema[categoryField].type)){
      errorMessages.push("Category Field "+errorIfNotInAuthorizedTypes);
    } 
    if (valueField==null){
      errorMessages.push("Value field must be defined");
    } else if (dataProvider.schema[valueField].type != 'number') {
      errorMessages.push("Value field must be a number field");
    }
    if (aggregationMethod==null){
      errorMessages.push("Aggregation method must be defined")
    }
  }
  if (selectionMethod == "list_fields"){
    if (serieField==null){
      errorMessages.push("You must fill-in serie field");
    }
    if (valueField1==null || valueField2 == null || valueField3 == null){
      errorMessages.push("You must fill-in field1, field2 and field3 which must be numeric fields");
    }
    if (valueField1 && dataProvider.schema[valueField1].type != 'number'){
      errorMessages.push("Value Field 1 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField2 && dataProvider.schema[valueField2].type != 'number'){
      errorMessages.push("Value Field 2 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField3 && dataProvider.schema[valueField3].type != 'number'){
      errorMessages.push("Value Field 3 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField3 != null && dataProvider.schema[valueField3].type != 'number'){
      errorMessages.push("Value Field 3 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField4 != null && dataProvider.schema[valueField4].type != 'number'){
      errorMessages.push("Value Field 4 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField5 != null && dataProvider.schema[valueField5].type != 'number'){
      errorMessages.push("Value Field 5 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField6 != null && dataProvider.schema[valueField6].type != 'number'){
      errorMessages.push("Value Field 6 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField7 != null && dataProvider.schema[valueField7].type != 'number'){
      errorMessages.push("Value Field 7 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField8 != null && dataProvider.schema[valueField8].type != 'number'){
      errorMessages.push("Value Field 8 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField9 != null && dataProvider.schema[valueField9].type != 'number'){
      errorMessages.push("Value Field 9 "+errorIfNotInAuthorizedTypes);
    }
    if (valueField10 != null && dataProvider.schema[valueField10].type != 'number'){
      errorMessages.push("Value Field 10 "+errorIfNotInAuthorizedTypes);
    }
  }
  if (errorMessages.length>0){
    canBeDisplayed=false;
  }

  // ----------------------------------------------------------------
  // Manage all options of the chart
  var options = {};
  options.chart = { height: chartHeight, type: 'radar', toolbar: {show: false}}
  if (chartWidth!=null){
    options.chart.width = chartWidth;
  }
  if (chartTitle!=null){
    options.title = { text: chartTitle }
  }
  options.yaxis = { }
  if (minAxis!=null){
    options.yaxis.min = minAxis;
  }
  if (maxAxis!=null){
    options.yaxis.max = maxAxis;
  }
  if (axisTickAmount!=null){
    options.yaxis.tickAmount = axisTickAmount;
  }
  options.dataLabels = { enabled: displayDataLabels , background: { enabled: true, borderRadius: 2}};
  options.theme = {palette : colorPalette };
  options.plotOptions = { radar : 
    {
      polygons: {
        strokeColor: '#e8e8e8',
        fill: {
            colors: ['#f8f8f8', '#fff']
        }
      }
    }
  };

  // Merge advanced chart options with options computed just before
  if (advancedChartOptions!=null){
    options = merge(options, advancedChartOptions)
  }

  // ----------------------------------------------------------------
  // Manage data of the chart
  options.series = [];
    
  function nvl(labelName,fieldName){
    return labelName==null?fieldName:labelName;
  }

  // Handle data when rows retrieved
  function handleData(rows){
    if (rows){
      options.labels = [];
      options.series = [];
      if (selectionMethod == "list_fields"){
          for (const element of rows){
            // Manage labels
            options.labels.push(nvl(labelField1,valueField1));
            options.labels.push(nvl(labelField2,valueField2));
            options.labels.push(nvl(labelField3,valueField3));
            if (valueField4 != null) options.labels.push(nvl(labelField4,valueField4));
            if (valueField5 != null) options.labels.push(nvl(labelField5,valueField5));
            if (valueField6 != null) options.labels.push(nvl(labelField6,valueField6));
            if (valueField7 != null) options.labels.push(nvl(labelField7,valueField7));
            if (valueField8 != null) options.labels.push(nvl(labelField8,valueField8));
            if (valueField9 != null) options.labels.push(nvl(labelField9,valueField9));
            if (valueField10 != null) options.labels.push(nvl(labelField10,valueField10));

            // Manage values
            let values = [];
            values.push(element[valueField1]);
            values.push(element[valueField2]);
            values.push(element[valueField3]);
            if (valueField4 != null) values.push(element[valueField4]);
            if (valueField5 != null) values.push(element[valueField5]);
            if (valueField6 != null) values.push(element[valueField6]);
            if (valueField7 != null) values.push(element[valueField7]);
            if (valueField8 != null) values.push(element[valueField8]);
            if (valueField9 != null) values.push(element[valueField9]);
            if (valueField10 != null) values.push(element[valueField10]);

            options.series.push({"name": element[serieField], "data": values});
          }
      } else if (selectionMethod == "category_value_fields"){
        if (serieField==null){
          options.series.push({"name":undefined,data: []});
        }
        for (const element of rows){
          
          //Check if serie exists and retrieve its index
          let indexSerie = 0;
          if (serieField!=null){
            let serieName = element[serieField];
            indexSerie = options.series.findIndex( el => el.name == serieName);
            if (indexSerie==-1){
              options.series.push({"name":serieName, "data": []});
              indexSerie = options.series.findIndex( el => el.name == serieName);
            }
          }
          
          //Now check if category exist
          let categoryName = element[categoryField];
          let categoryIndex = options.labels.findIndex( el => el == categoryName);
          if (categoryIndex == -1){
            options.labels.push(categoryName);
            categoryIndex = options.labels.findIndex( el => el == categoryName);
            options.series[indexSerie].data.push(null);
          }

          // Add values 
          let currVal = options.series[indexSerie].data[categoryIndex];
          let newVal = element[valueField];
          if (currVal == null){
            newVal = newVal;
          } else {
            if (aggregationMethod=="min"){
              if (newVal>currVal) newVal = currVal;
            } else if (aggregationMethod=="max"){
              if (newVal<currVal) newVal = currVal;
            } else if (aggregationMethod=="first"){
              newVal = currVal;
            } else if (aggregationMethod=="last"){
              newVal = newVal;
            }
          }
          options.series[indexSerie].data[categoryIndex] = newVal;
        }
      }
    } else {
      canBeDisplayed = false;
      errorMessages.push("No data");
    }
  }

  // Load data and manage them
  $: rows = $loading
    ? new Array(dataProvider?.limit > 20 ? 20 : dataProvider?.limit).fill({})
    : dataProvider?.rows;
  $: if (dataProvider?.rows && dataProvider?.rows.length > 0){
      handleData(dataProvider?.rows);
  }
</script>

{#if canBeDisplayed}
  <div use:chart={options} />
{:else}
<!-- Display error message of bad configuration -->
  <div class="spectrum-InLineAlert spectrum-InLineAlert--notice">
    <div class="spectrum-InLineAlert-header">
      Bubble chart configuration error
      <svg class="spectrum-Icon spectrum-Icon--sizeM spectrum-InLineAlert-icon" focusable="false" aria-hidden="true">
        <use xlink:href="#spectrum-icon-18-Alert" />
      </svg>
    </div>
    <div class="spectrum-InLineAlert-content">
      <ul>
        {#each errorMessages as message}
          <li>{message}</li>
        {/each}
      </ul>
    </div>
  </div>
{/if}