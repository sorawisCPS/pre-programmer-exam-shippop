<template>
  <v-app id="inspire">
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            cols="12"
            sm="8"
            md="8"
          >
            <v-card class="elevation-12">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Search Algorithms</v-toolbar-title>
                <v-spacer />
              </v-toolbar>
              <v-card-text>
                <v-form ref="form">
                  <v-row>
                    <v-col
                      cols="12"
                      sm="12"
                      md="12"
                    >
                      <v-text-field
                          v-model="list"
                          :rules="listRules"
                          label="List"
                          required
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col
                      cols="6"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                          v-model="search"
                          :rules="searchRules"
                          label="ค้นหา"
                          required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="6"
                      sm="5"
                      md="5"
                    >
                      <v-btn color="warning" large @click="doSearch">ค้นหา</v-btn>
                    </v-col>
                  </v-row>
                  <v-row justify="center">
                    <v-col
                      cols="6"
                      sm="6"
                      md="6"
                      style="text-align:center;"
                    >
                      <h1>ประเภทการค้นหา</h1>
                    </v-col>
                  </v-row>
                  <v-row justify="center">
                    <v-col
                      cols="6"
                      sm="6"
                      md="6"
                    >
                      <v-select
                        :items="items"
                        v-model="searchAlgor"
                        outlined
                      ></v-select>
                    </v-col>
                  </v-row>
                  <v-row justify="center">
                     <v-col
                      cols="10"
                      sm="10"
                      md="10"
                    >
                    <h2>ผลลัพธ์</h2>
                    </v-col>
                  </v-row>
                  <v-row justify="center">
                     <v-col
                      cols="10"
                      sm="10"
                      md="10"
                    >
                      <v-textarea
                        class="centered-input"
                        outlined
                        name="input-7-4"
                        :value="result"
                        no-resize
                        readonly
                        auto-grow
                      ></v-textarea>
                    </v-col>
                  </v-row>
                </v-form>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  export default {
    data() {
        return {
            result : "",
            list : "",
            listRules: [
                v => !!v || 'List is required',
                v => /^\d+(,\d+)*$/.test(v) || 'Incorrect format',
            ],
            search: '',
            searchRules: [
                v => !!v || 'Search keyword is required',
                v => /^[0-9]*$/.test(v) || 'Only a number without decimals is accept',
            ],
            searchAlgor : 1,
            items: [
              { text: 'Linear Search', value: 1},
              { text: 'Binary Search', value: 2},
              { text: 'Jump Search', value: 3}
            ]
        }
    },
    methods: {
      doSearch() {
        if (this.$refs.form.validate()) {
          // console.log(this.searchAlgor)
          if(this.searchAlgor == 1){
            this.doLinearSearch(this.list,this.search)
          }else if(this.searchAlgor == 2){
            this.doBinarySearch(this.list,this.search)
          }else if(this.searchAlgor == 3){
            this.doJumpSearch(this.list,this.search)
          }
        }
      },
      doLinearSearch(list,search) {
        let textResult = ""
        textResult = "List : ["+list+"]"
        textResult += "\n"
        textResult += "Search : "+search
        textResult += "\n"
        textResult += "Result ::: "
        textResult += "\n"

        const arrayList = list.split(',')
        let counter = 1
        let isFound = false
        arrayList.forEach(e => {
          if(!isFound){
            if(search !== e){
              textResult += "Round : "+counter+" ===> "+search+" != "+e
              textResult += "\n"
            }else{
              textResult += "Round : "+counter+" ===> "+search+" = "+e+" found!!"
              isFound = true
            }
            counter++
          }
        });
        
        if(!isFound) {
          textResult += "Result :: not found!!"
        }
        this.result = textResult
      },
      doBinarySearch(list,search) {
        let textResult = ""
        textResult = "List : ["+list+"]"
        textResult += "\n"
        textResult += "Search : "+search
        textResult += "\n"

        let arrayList = list.split(',')
        let sortedArray = arrayList.sort((a, b) => a - b)
        let stringSorted = sortedArray.join(",",sortedArray)
        let counter = 1
        let isFound = false

        textResult += "Sorted List : ["+stringSorted+"]"
        textResult += "\n"
        textResult += "Result ::: "
        textResult += "\n"
        this.result = textResult
        this.recursiveBinarySearch(sortedArray, search, 0, sortedArray.length-1,counter)
      },
      doJumpSearch(list,search) {
        let textResult = ""
        textResult = "List : ["+list+"]"
        textResult += "\n"
        textResult += "Search : "+search
        textResult += "\n"
        
        let arrayList = list.split(',')
        let sortedArray = arrayList.sort((a, b) => a - b)
        let stringSorted = sortedArray.join(",",sortedArray)
        let counter = 1

        textResult += "Sorted List : ["+stringSorted+"]"
        textResult += "\n"
        this.result = textResult


        var length = sortedArray.length;
        // Finding block size to be jumped 
        var step = Math.floor(Math.sqrt(length));
        var index = Math.min(step, length)-1;

        this.result += "Jump size :"+(step)
        this.result += "\n"
        this.result += "Result ::: "
        this.result += "\n"

        // Finding the block where element is 
        // present (if it is present) 
        var lowerBound = 0;
        while (parseInt(sortedArray[Math.min(step, length)-1]) < parseInt(search))
        {
          this.result += "==== Round :"+counter+ "===="
          this.result += "\n"
          this.result += "Jump to index :"+(step)
          this.result += "\n"
          this.result += "Index :"+(step)+", Value :"+sortedArray[step]
          this.result += "\n"
          counter++
          lowerBound = step;
          step += step;
          if (lowerBound >= length){
            this.result += "Result :: not found!!"
            return false
          }
        }
        // Doing a linear search for x in block 
        // beginning with prev. 
        var upperBound = Math.min(step, length);
        while (parseInt(sortedArray[lowerBound]) < parseInt(search))
        {
          this.result += "==== Round "+counter+" ===="
          this.result += "\n"
          this.result += "Index :"+lowerBound+ ", Value : "+sortedArray[lowerBound]+" is smaller than "+search
          this.result += "\n"
          counter++
          lowerBound++;
          this.result += "Move to Index :"+lowerBound+ ", Value :"+sortedArray[lowerBound]
          this.result += "\n"
          // If we reached next block or end of 
          // array, element is not present. 
          if (lowerBound == upperBound){
            this.result += "Result :: not found!!"
            return false

          }
        }
        // If element is found
        if (parseInt(sortedArray[lowerBound]) == parseInt(search)){
          // this.result += "xxxx Round "+counter+" xxxx"
          // this.result += "\n"
          this.result += "Result :: found!!"
          return true
        }else{
          this.result += "Result :: not found!!"
          return false
        }
      },
      recursiveBinarySearch(arr, x, start, end,counter) {   
          this.result += "==== Round: "+counter+" ====\n"
          // Base Condition 
          if (start > end){
            this.result += "Result :: not found"
            return false
          } 
        
          // Find the middle index 
          let mid=Math.floor((start + end)/2); 
          this.result += 'Mid ==> '+arr[mid]
          this.result += '\n'

          // Compare mid with given key x 
          if (parseInt(arr[mid])===parseInt(x)){
            this.result += 'Result :: Found!!'
            return true
          }
          
          // If element at mid is greater than x, 
          // search in the left half of mid 
          if(parseInt(arr[mid]) > parseInt(x)) {
            let slice = arr.slice(start,mid)
            let toString = slice.join(',',slice)
            this.result += "Mid("+arr[mid]+') is greater than '+x
            this.result += '\n'
            this.result += "Current List : ["+toString+"]\n"
            counter += 1
            return this.recursiveBinarySearch(arr, x, start, mid-1,counter); 
          }else{
            // If element at mid is smaller than x, 
            // search in the right half of mid 
            let slice = arr.slice(mid+1,end+1)
            let toString = slice.join(',',slice)
            this.result += "Mid("+arr[mid]+') is smaller than '+x
            this.result += '\n'
            this.result += "Current List : ["+toString+"]\n"
            counter += 1
            return this.recursiveBinarySearch(arr, x, mid+1, end,counter); 
          }
      } 
    }
  }
</script>

<style>
.centered-input textarea {
  text-align: center
}
</style>