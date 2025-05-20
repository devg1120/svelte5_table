<script>

 import SvelteEditTable from './lib/SvelteEditTable.svelte';

 function table_json_to_array(json) {
    let items = JSON.parse(json);
    var index = {};
    var result = [];

    result.push([]);
    for (var key in items[0].index) {
      index[key] = items[0].index[key];
    }
    for (var key in items[0].title) {
      result[result.length - 1].push(items[0].title[key]);
    }
    var i = 0;
    while (i < items.length) {
      if (items[i].fields) {
        result.push([]);
        for (var key in index) {
          result[result.length - 1].push(items[i].fields[key]);
        }
      }
      i++;
    }
    var index = {};
    var result = [];

    result.push([]);
    for (var key in items[0].index) {
      index[key] = items[0].index[key];
    }
    for (var key in items[0].title) {
      result[result.length - 1].push(items[0].title[key]);
    }
    var i = 0;
    while (i < items.length) {
      if (items[i].fields) {
        result.push([]);
        for (var key in index) {
          result[result.length - 1].push(items[i].fields[key]);
        }
      }
      i++;
    }
    return result;
  }

let table1;
let table2;
let table3;
let table4;
let table5;
let table6;

     let json_data1 = `
        [
           {"index":["id","first_name","last_name","age"],
            "type" :{"id":"int","first_name":"string","last_name":"string","age":"int"},
            "title":{"id":"Id","first_name":"First Name","last_name":"Last Name","age":"Age"},
            "pk":345,
            "update":""
            },
           {"fields":{"id":1,"last_name":"Prasanth","age":24},
            "model":"hug.tree",
            "pk":345
            },
           {"fields":{"id":2,"first_name":"Saravana","last_name":"vel","age":31},
            "model":"hug.tree",
            "pk":484
            },
           {"fields":{"id":3,"first_name":"Kumar","last_name":"KG","age":64},
            "model":"hug.tree",
            "pk":484
            }
         ]
   `;

       let f_data3 = "<p>";
        f_data3 +=
          "<span style='color: rgb(255, 0, 0)'><strong>SunEditor</strong></span>";
        f_data3 += "</br>";
        f_data3 +=
          "<span style='color: rgb(34, 144, 255)'><strong>SunEditor</strong></span>";
        f_data3 += "</br>";
        //f_data3 += "<span style='color: rgb(95,158,160)'><strong>SunEditor</strong></span>";
        f_data3 += "<span style='color: rgb(95,158,160)'>";
        f_data3 += " <strong>Sun</strong>";
        f_data3 += "</span>";
        f_data3 += "<span style='font-size: 20px'>";
        f_data3 += " <span style='color: rgb(0,0,205)'>";
        f_data3 += "   <strong>Editor</strong>";
        f_data3 += " </span>";
        f_data3 += "</span>";
        f_data3 += "</p>";

        let json_data2 = `
        [
           {"index":["id","first_name","last_name"],
            "type":{"id":"int","first_name":"string","last_name":"string"},
            "title":{"id":"Id","first_name":"First Name","last_name":"Last Name"},
            "pk":345,
            "update":""
            },
	   {"fields":{"id":1,"first_name":"Gowri","last_name":"${f_data3}"},
            "model":"hug.tree",
            "pk":345
            },
	   {"fields":{"id":2,"first_name":"Saravana","last_name":"${f_data3}"},
            "model":"hug.tree",
            "pk":484
            },
           {"fields":{"id":3,"first_name":"Kumar","last_name":"KG"},
            "model":"hug.tree",
            "pk":484
            },
           {"fields":{"id":4,"first_name":"Syo","last_name":"Gusa"},
            "model":"hug.tree",
            "pk":484
            }
         ]
   `;
        let json_data3 = `
        [
           {"index":["A","B","C","D","E"],
            "type":{"A":"int","B":"int","C":"int","D":"int","E":"int"},
            "title":{"A":"A","B":"B","C":"C","D":"D","E":"E"},
	    "freeze": { "c": 0, "r":0},
            "span": [
                     {"r": 2, "c": 1, "rowspan":2, "colspan": 2}
                    ],
            "update":""
            },
           {"fields":{"A":1,"B":2,"C":3,"D":4,"E":5}
            },
           {"fields":{"A":11,"B":12,"C":13,"D":14,"E":15}
            },
           {"fields":{"A":21,"B":22,"C":23,"D":24,"E":25}
            },
           {"fields":{"A":31,"B":32,"C":33,"D":34,"E":35}
            }
         ]
   `;

     function ost(obj) {
          //return JSON.stringify(obj,null,'\t');
          return JSON.stringify(obj, null, "");
        }
        let colm = [];
        for (let n = 0; n < 26; n++) {
          var char = String.fromCharCode(97 + n).toUpperCase();
          //console.log(char);
          colm.push(char);
        }
        //console.log(colm);

        let data = [];

        let index = [];
        for (let i = 0; i < colm.length; i++) {
          index.push(colm[i]);
        }
        let type = {};
        for (let i = 0; i < colm.length; i++) {
          type[colm[i]] = "int";
        }
        let title = {};
        for (let i = 0; i < colm.length; i++) {
          title[colm[i]] = colm[i];
        }
        let head = {
          index: index,
          type: type,
          title: title,
          freeze: { c: 0, r: 0 },
          update: "",
        };
        data.push(head);
        let ymax = 15;
        for (let y = 0; y < ymax; y++) {
          let fields = {};
          for (let x = 0; x < colm.length; x++) {
            fields[colm[x]] = String(y + 1) + "-" + String(x + 1);
          }
          let fields_data = { fields: fields };
          data.push(fields_data);
        }
        //console.log(ost(data));
        data[0].span = [
                         {"r": 4, "c": 2, "rowspan":4, "colspan": 3}
                       ];
        let json_data4 = ost(data);
        delete data[0].span;
        data[0].freeze = { c: 2, r: 2 };
        let json_data5 = ost(data);

      function json_data_dump(data) {
         console.dir($state.snapshot(data));
      }

      function table_data_dump(table_element) {
         let table = table_element.get_table();
         //console.dir(table);
         console.dir(table.get_table_data());
      }

      //let save = () => {
      function save() {
        //let content = table3.editor.getText();
	console.log(table5);
	//console.log(table2.get_table());
	let table = table5.get_table();
        console.log(table.table_data);
        //table2.editor.readOnly(true);
        //table2.editor.disable()

        //var td = document.createElement("td");
        //td.innerHTML = content;

        //table2.editor.destroy();
        //console.log(table2.editting_td);
        //table2.editting_td.innerHTML = content;
        //table2.editting_td.style.display = "";
      };

      function setdata() {
	//console.log(data5[2]["fields"]["A"])
	//console.log(data1[2]["fields"])
	data5[2]["fields"]["A"] = "9-9"
	data1[2]["fields"]["id"] = "9-9"
	//console.log(data5[2]["fields"]["A"])
      };

  function json_syntax_check(json) {
    //console.log(json);
    try {
      const data_ = JSON.parse(json);
    } catch (error) {
      console.log(error);
      return false;
    }   
    return true;
  }

    let data1 = $state();
    let data2 = $state();
    let data3 = $state();
    let data4 = $state();
    let data5 = $state();

    if (!json_syntax_check(json_data1))  { console.log("json_data1 ERROR") ;}
    if (!json_syntax_check(json_data2))  { console.log("json_data2 ERROR") ;}
    if (!json_syntax_check(json_data3))  { console.log("json_data3 ERROR") ;}
    if (!json_syntax_check(json_data4))  { console.log("json_data4 ERROR") ;}
    if (!json_syntax_check(json_data5))  { console.log("json_data5 ERROR") ;}

    data1 = JSON.parse(json_data1);
    data2 = JSON.parse(json_data2);
    data3 = JSON.parse(json_data3);
    data4 = JSON.parse(json_data4);
    data5 = JSON.parse(json_data5);

</script>
<style>

table tr td,
table tr th {
  word-wrap: break-word;
  border: 0px solid #fff; 
  min-width: 130px;
  padding: 5px;
  position: relative;
  text-align: left;
  box-sizing: border-box;
  min-width: 30px;
}

</style>
<main>
  <div>
    
  <h1>Svelte5 Editable Table</h1>
  <table><tbody>
  <tr>
   <td>table1</td> 
   <td><button onclick={() => json_data_dump(data1)}>dump</button></td>
  </tr>
  </tbody></table>
  <div class="edittable">
  <SvelteEditTable  bind:json_data={data1} container_id="container1"   table_width="500px" bind:this={table1} />
  </div>

  <table><tbody>
  <tr>
   <td>table2</td> 
   <td><button onclick={() => json_data_dump(data2)}>dump</button></td>
  </tr>
  </tbody></table>
  <div class="edittable">
  <SvelteEditTable  bind:json_data={data2} container_id="container2"  table_width="500px" bind:this={table2} />
  </div>
  
  <table><tbody>
  <tr>
   <td>table3</td> 
   <td><button onclick={() => json_data_dump(data3)}>json_data</button></td>
   <td><button onclick={() => table_data_dump(table3)}>table_data</button></td>
  </tr>
  </tbody></table>
  <div class="edittable">
  <SvelteEditTable  bind:json_data={data3} container_id="container3"  table_width="500px" bind:this={table3} />
  </div>
  
  <table><tbody>
  <tr>
   <td>table4</td> 
   <td><button onclick={() => json_data_dump(data4)}>dump</button></td>
  </tr>
  </tbody></table>
  <div class="edittable">
  <div class="scroll-table-wrapper">
  <SvelteEditTable  bind:json_data={data4} container_id="container4"   table_width="400%" scrollable=true bind:this={table4} />
  </div>
  </div>

  <table><tbody>
  <tr>
   <td>table5</td> 
   <td><button onclick={() => json_data_dump(data5)}>dump</button></td>
  </tr>
  </tbody></table>
  <div class="edittable">
  <div class="scroll-table-wrapper">
  <SvelteEditTable  bind:json_data={data5} container_id="container5"   table_width="400%" scrollable=true bind:this={table5} />
  </div>
  </div>

  <table><tbody>
  <tr>
   <td>table6</td> 
   <td><button onclick={() => json_data_dump(data5)}>dump</button></td>
  </tr>
  </tbody></table>
  <div class="edittable">
  <div class="scroll-table-wrapper">
  <SvelteEditTable  bind:json_data={data5} container_id="container6"   table_width="400%" scrollable=true bind:this={table6} />
  </div>
  </div>

  <button onclick={save}> save </button> &nbsp;  
  
  <button onclick={setdata}> set </button> 
 
</main>


