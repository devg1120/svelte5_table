<script>

 import SvelteEditTable from './lib/SvelteEditTable.svelte';
let table1;
let table2;
let table3;
let table4;
let table5;

     let json_data = `
        [
           {"index":{"id":"int","first_name":"string","last_name":"string","age":"int"},
            "title":{"id":"Id","first_name":"First Name","last_name":"Last Name","age":"Age"},
            "pk":345
            },
           {"fields":{"id":1,"first_name":"Gowri","last_name":"Prasanth","age":24},
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
           {"index":{"id":"int","first_name":"string","last_name":"string"},
            "title":{"id":"Id","first_name":"First Name","last_name":"Last Name"},
            "pk":345
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
           {"index":{"A":"int","B":"int","C":"int","D":"int","E":"int"},
            "title":{"A":"A","B":"B","C":"C","D":"D","E":"E"},
	    "freeze": { "c": 0, "r":0}
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
        let index = {};
        for (let i = 0; i < colm.length; i++) {
          index[colm[i]] = "int";
        }
        let title = {};
        for (let i = 0; i < colm.length; i++) {
          title[colm[i]] = colm[i];
        }
        let head = {
          index: index,
          title: title,
          freeze: { c: 0, r: 0 },
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
        let json_data4 = ost(data);
        data[0].freeze = { c: 2, r: 2 };
        let json_data5 = ost(data);


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
	//console.log(table2.get_table());
	let table = table5.get_table();
        table.table_data[3][3] = "<p>9999</p>";
      };

</script>

<main>
  <div>
    
  <h1>Svelte5 Editable Table</h1>

  <SvelteEditTable  json_data={json_data} container_id="container1"   table_width="500px" bind:this={table1} />
  <br/>
  <SvelteEditTable  json_data={json_data2} container_id="container2"  table_width="500px" bind:this={table2} />
  <br/>
  <SvelteEditTable  json_data={json_data3} container_id="container3"  table_width="500px" bind:this={table3} />
  <br/>

  <div class="scroll-table-wrapper">
  <SvelteEditTable  json_data={json_data4} container_id="container4"   table_width="400%" scrollable=true bind:this={table4} />
  </div>
  <br/>

  <div class="scroll-table-wrapper">
  <SvelteEditTable  json_data={json_data5} container_id="container5"   table_width="400%" scrollable=true bind:this={table5} />
  </div>
  <br/>

  <button onclick={save}> save </button> &nbsp;  
  
  <button onclick={setdata}> set </button> 
 
</main>


