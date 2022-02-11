<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-4">
        <b-form-input id="spiralInput" v-model="input" :number="true" placeholder="Enter a number"></b-form-input>
        <b-button variant="primary" @click="generateSpiral">Calculate</b-button>
      </div>
    </div>
    <div class="row justify-content-center">
      <div v-if="showOutput" class="col-4">
        <label>Output:</label>
        <table style="width: 100%">
          <tbody>
            <tr v-for="(row, index1) in output" :key="index1">
              <td v-for="(col, index2) in row" :key="index2">
                {{output[index1][index2]}}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SpiralGenerator',
  props: {
    msg: String
  },
  data: () => {
    return {
      input: null,
      showOutput: false,
      output: []
    } 
  },
  methods: {
    generateSpiral(){
      var square = Math.ceil(Math.sqrt(this.input));
      //Set the output to a 2D array meeting the necessary dimensions based on the entry (square root ceiling)
      this.output = new Array(square);
      for(var l = 0; l < square; l++){
        this.output[l] = new Array(square);
      }
      //Set base cases
      //Place 0 at the center
      //if the input is greater than 0, place 1 to the right of 0
      var counter = 1;
      var center = Math.ceil(square/2)-1
      this.output[center][center] = 0;
      if(this.input != 0){
        this.output[center][center+1] = 1;
      }
      //Set starting entry points
      var row = center;
      var column = center+1;
      var mod = 1;
      for(var i = 1; i<square; i++){
        //populate a full row and column as long as we have not yet hit the input
        //Row entry
        for(var k = 1; k <=i; k++){
          row += mod;
          counter++;
          this.output[row][column] = counter;
          if(counter == this.input){
            break;
          }
        }
        if(counter == this.input){
          break;
        }
        //Switch the direction of entry (down -> up) (left -> right)
        mod = 0-mod;
        //Column entry
        for(var j = 1; j <=i; j++){
          column += mod;
          counter++;
          this.output[row][column] = counter;
          if(counter == this.input){
            break;
          }
        }
        //If we haven't hit the limit yet, start a new column, expanding the square
        if(column != 0 && column != square -1 && counter!=this.input){
          column += mod;
          counter++;
          this.output[row][column] = counter;
          if(counter == this.input){
            break;
          }
        }
        if(counter == this.input){
          break;
        }
      }
      //Snip unused edges off of the output array
      if(this.output[this.output.length-1][this.output[this.output.length-1].length-1] == undefined){
        this.output.splice(this.output.length-1, 1);
      }
      
      if(this.output[this.output.length-1][0] == undefined){
        for(var m = 0; m<this.output.length; m++){
          this.output[m].splice(0, 1);
        }
      }
      
      if(this.output[0][0] == undefined){
        this.output.splice(0, 1);
      }
      //Display the array
      this.showOutput = true;
    }
  }
}
</script>
