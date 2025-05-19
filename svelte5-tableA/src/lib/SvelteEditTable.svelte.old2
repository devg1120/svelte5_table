<script>   
import "./styles.css";  

import "suneditor/dist/css/suneditor.min.css";
// import 'suneditor/assets/css/suneditor.css'
// import 'suneditor/assets/css/suneditor-contents.css'

import suneditor from "suneditor";
import plugins from "suneditor/src/plugins";
import image from "suneditor/src/plugins/dialog/link";
import list from "suneditor/src/plugins/submenu/list";
import { font, video } from "suneditor/src/plugins";


export class Table {
  col_element;
  next_element;
  y_cursorStart;
  x_cursorStart;
  y_dragStart;
  x_dragStart;
  width;
  height;
  th_width;
  next_width;
  next_height;
  resize;
  resize_left;
  table_wt;
  resizeCheck;
  dragging_row;
  table;
  table_data;

  constructor(container_id, width) {
    this.diff_update = true 
    this.container_id = container_id;
    this.table_width = width;
    this.menu_list = [
      ["next insert row", this.next_insert_row],
      ["prev insert row", this.prev_insert_row],
      ["left insert col", this.left_insert_col],
      ["right insert col", this.right_insert_col],
      ["freeze on " , this.freeze_on],
      ["freeze off ", this.freeze_off],
      ["data dump", this.data_dump],
      ["data save", this.data_save],
    ];
    this.td_select_drag_start = false;
    this.editor = null;
    this.editting_td = null;
    this.freeze_decoded = { c: 0, r: 0 };
  }

  get_table_data() {
     return this.table_data;
  }

  table_json_set_option(items) {
    //let items = JSON.parse(json);
    if (items[0].freeze) {
      this.freeze_decoded = items[0].freeze;
      //console.log(this.container_id, "freeze", this.freeze_decoded);
    }
  }
/*
  table_json_to_array2(json) {
    let items = JSON.parse(json);
    var index = {};
    //var result = [];

    this.table_data.push([]);
    for (var key in items[0].index) {
      index[key] = items[0].index[key];
    }
    for (var key in items[0].title) {
      this.table_data[this.table_data.length - 1].push(items[0].title[key]);
    }
    var i = 0;
    while (i < items.length) {
      if (items[i].fields) {
        this.table_data.push([]);
        for (var key in index) {
          this.table_data[this.table_data.length - 1].push(items[i].fields[key]);
        }
      }
      i++;
    }
    var index = {};

    this.table_data.push([]);
    for (var key in items[0].index) {
      index[key] = items[0].index[key];
    }
    for (var key in items[0].title) {
      this.table_data[this.table_data.length - 1].push(items[0].title[key]);
    }
    var i = 0;
    while (i < items.length) {
      if (items[i].fields) {
        this.table_data.push([]);
        for (var key in index) {
          this.table_data[this.table_data.length - 1].push(items[i].fields[key]);
        }
      }
      i++;
    }
    //return result;
  }
*/

  table_json_to_array_old(items) {
    //let items = JSON.parse(json);
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

  table_json_to_array(items) {
    //let items = JSON.parse(json);
/*
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
*/
    //var index = {};
    var index = [];
    var result = [];

    result.push([]);
    //for (var key in items[0].index) {
    //  index[key] = items[0].index[key];
    //}
    index = items[0].index;
    //for (var key in items[0].title) {
    for (const key of index) {
      result[result.length - 1].push(items[0].title[key]);
    }
    var i = 0;
    while (i < items.length) {
      if (items[i].fields) {
        result.push([]);
        //for (var key in index) {
        for (const key of index) {
          result[result.length - 1].push(items[i].fields[key]);
        }
      }
      i++;
    }
    return result;
  }

  row_dragging_start = (e) => {
    this.dragging_row = e.target;
  };

  row_dragging_over = (e) => {
    e.preventDefault();

    let children = Array.from(e.target.parentNode.parentNode.children);

    if (
      children.indexOf(e.target.parentNode) >
      children.indexOf(this.dragging_row)
    )
      e.target.parentNode.after(this.dragging_row);
    else e.target.parentNode.before(this.dragging_row);
  };

  table_build(table_data) {
    this.table_data = table_data;
    var menu = document.createElement("div");
    menu.id = "context-menu";
    var table = document.createElement("table");
    table.style.width = this.table_width;
    this.table = table;
    table.id = "table_resize";
    table.contentEditable = true;
    for (var i = 0; i < table_data.length; i++) {
      var tr = document.createElement("tr");
      //tr.setattribute("draggable", true)                           // row drag move
      //tr.addeventlistener("dragstart", this.row_dragging_start);
      //tr.addeventlistener("dragover", this.row_dragging_over);
      //tr.style += this.freezeStyle_tr(i);
      for (var j = 0; j < table_data[0].length; j++) {
        if (i === 0) {
          var th = document.createElement("th");
          th.textContent = table_data[i][j];
          tr.appendChild(th);
        } else {
          var td = document.createElement("td");
          //td.textContent = table_data[i][j];
          td.innerHTML = table_data[i][j];
          td.classList.add("noselect");
          tr.appendChild(td);
        }
      }
      table.appendChild(tr);
    }
    this.container.appendChild(menu);
    this.container.appendChild(table);
  }

  table_update(table_data) {

    if (this.diff_update) {
      for (var i = 0; i < table_data.length; i++) {
        for (var j = 0; j < table_data[0].length; j++) {
          if (i === 0) {
            if (table_data[i][j] != this.table_data[i][j]) {
              var th = this.table.querySelector("#TH_0_" + String(j));
              th.textContent = table_data[i][j];
            }
          } else {
            if (table_data[i][j] != this.table_data[i][j]) {
              var td = this.table.querySelector("#TD_" + String(i) + "_" + String(j));
              td.innerHTML = table_data[i][j];
            }
          }
        }
      }
    } else {
      for (var i = 0; i < table_data.length; i++) {
        //var tr = document.createElement("tr");
        for (var j = 0; j < table_data[0].length; j++) {
          if (i === 0) {
            var th = this.table.querySelector("#TH_0_" + String(j));
            th.textContent = table_data[i][j];
          } else {
            var td = this.table.querySelector("#TD_" + String(i) + "_" + String(j));
            //td.textContent = table_data[i][j];
            td.innerHTML = table_data[i][j];
          }
        }
      }
    }
    this.table_data = table_data;
  }

  table_freeze() {
    for (let i = 0; i < this.table.childNodes.length; i++) {
      if (this.table.childNodes[i].nodeName == "TR") {
        let tr = this.table.childNodes[i];
        this.freezeStyle_tr_set(tr, i);
        for (let j = 0; j < tr.childNodes.length; j++) {
          if (
            tr.childNodes[j].nodeName == "TH" ||
            tr.childNodes[j].nodeName == "TD"
          ) {
            this.freezeStyle_td_set(tr.childNodes[j], j, i);
          }
        }
      }
    }
  }

  table_freeze_old() {
    for (let i = 0; i < this.table.childNodes.length; i++) {
      if (this.table.childNodes[i].nodeName == "TR") {
        let tr = this.table.childNodes[i];
        tr.style = this.freezeStyle_tr(i);
        for (let j = 0; j < tr.childNodes.length; j++) {
          if (tr.childNodes[j].nodeName == "TH") {
            tr.childNodes[j].style = this.freezeStyle_td(j, i);
          }
          if (tr.childNodes[j].nodeName == "TD") {
            tr.childNodes[j].style = this.freezeStyle_td(j, i);
          }
        }
      }
    }
  }

  json_syntax_check(json) {
    //console.log(json);
    try {
      const data_ = JSON.parse(json);
    } catch (error) {
      console.log(error);
      return false;
    }
    return true;
  }

  init(json) {
    //if (!this.json_syntax_check(json)) {
    //  console.log("ERROR", this.container_id);
    //  return;
    //}

    //this.container = document.getElementById(this.container_id);
    this.table_json_set_option(json);
    this.table_data = this.table_json_to_array(json);
    //this.table_json_to_array2(json);

    this.table_build(this.table_data);

    this.y_cursorStart = 0;
    this.x_cursorStart = 0;
    this.y_dragStart = false;
    this.x_dragStart = false;
    this.next_width = undefined;

    this.table = this.container.querySelector("#table_resize");

    this.contextMenu = this.container.querySelector("#context-menu");
    this.table_th = this.table.getElementsByTagName("th");
    this.table_tr = this.table.getElementsByTagName("tr");

    this.bodyRect = document.body.getBoundingClientRect();
    this.tableRect = this.table.getBoundingClientRect();

    this.container.style.position = "relative";

    this.select_td = [null, null];
    this.tds = this.table.querySelectorAll("td");

    this.table.addEventListener("keydown", (event) => {
      event.preventDefault();
      event.stopPropagation();
      //https://qiita.com/nishimachikid/items/aca5b037af623e26929e
      //console.log("key:", this.container_id, event.key);
         if (event.key == "ArrowDown") {
	     console.log("down");
	     if (!event.shiftKey) {
	        this.select_move("down");
	     } else {
	        this.select_expand("down");
	     }
         } else if (event.key == "ArrowUp") {
	     console.log("up");
	     if (!event.shiftKey) {
	        this.select_move("up");
	     } else {
	        this.select_expand("up");
	     }
         } else if (event.key == "ArrowRight") {
	     console.log("right");
	     if (!event.shiftKey) {
	        this.select_move("right");
	     } else {
	        this.select_expand("right");
	     }
         } else if (event.key == "ArrowLeft") {
	     console.log("left");
	     if (!event.shiftKey) {
	        this.select_move("left");
	     } else {
	        this.select_expand("left");
	     }
         } else if (event.key == "Escape") {
	     console.log("esc");
	     this.select_escape();
         } else if (event.key == "Control") {
	     console.log("ctl");
         } else if (event.key == "Alt") {
	     console.log("alt");
         } else if (event.key == "Shift") {
	     console.log("shift");
         } else if (event.key == "Enter") {
	     console.log("enter");
         } else if (event.key == "Backspace") {
	     console.log("bs");
         } else if (event.key == "Delete") {
	     console.log("delete");
	 }
    });

    this.table.addEventListener("contextmenu", (event) => {
      event.preventDefault();
      event.stopPropagation();

      const { clientX: mouseX, clientY: mouseY } = event;

      this.contextMenu.classList.remove("visible");

      this.contextMenu.style.top = `${mouseY}px`;
      this.contextMenu.style.left = `${mouseX}px`;

      setTimeout(() => {
        this.contextMenu.classList.add("visible");
      });
    });

    this.table.addEventListener("click", (e) => {
      if (e.target.offsetParent != this.contextMenu) {
        this.contextMenu.classList.remove("visible");
      }
    });

    this.container.addEventListener("resize", (e) => {
      console.log("resize", this.container_id);
    });

    this.menu_setup();
    this.table_setid();
    this.setTdWidth(this.table);
    this.createResizeColDiv();
    this.createResizeRowDiv();
    this.initEvents(this.table_th);
  }

  update(json) {
    let data = this.table_json_to_array(json);
    this.table_update(data);
  }

  table_rows() {
    return this.table.rows.length;
  }

  table_cells() {
    return this.table.rows[0].cells.length;
  }

  next_insert_row = () => {
    console.log("next_insert_row")
    this.contextMenu.classList.remove("visible");
    if (this.select_td[0] == null) {
      return;
    }

    let param = this.select_td[0].id.split("_");
    let row_num = parseInt(param[1]);
    let tr = this.table.insertRow(row_num + 1);
    tr.style.height = "35px";
    for (let c = 0; c < this.table_cells(); c++) {
      let newCell = tr.insertCell();
      let newText = document.createTextNode("");
      newCell.appendChild(newText);
    }
    this.table_setid();
    json_data[0].update = "next-insert-row " + row_num;
    json_data[0].update_source = this.container_id;;
    //let data = json_data[0]["index"]  ;
    //for (var key in data) {
    //      data[key] = "-";
    //}
    let index = json_data[0]["index"]  ;
    let data = json_data[0]["type"]  ;
    for (const key of index) {
          data[key] = "-";
    }
   let fields =  { "fields": data };
   Array.prototype.splice.apply(json_data,[row_num + 1,0].concat([fields]));
   console.dir(json_data);
   //this. createResizeRowDiv();
   var xDiv = document.createElement("div");
   xDiv.className = "x_resize tb_resize";
   xDiv.setAttribute("data-resizerow", json_data.length);
   this.container.append(xDiv);

  };

  prev_insert_row = () => {
    console.log("prev_insert_row")
    this.contextMenu.classList.remove("visible");
    if (this.select_td[0] == null) {
      return;
    }
    let param = this.select_td[0].id.split("_");
    let row_num = parseInt(param[1]);
    let tr = this.table.insertRow(row_num);
    tr.style.height = "35px";
    for (let c = 0; c < this.table_cells(); c++) {
      let newCell = tr.insertCell();
      let newText = document.createTextNode("");
      newCell.appendChild(newText);
    }
    this.table_setid();

    json_data[0]["update"] = "prev-insert-row " + row_num;
    json_data[0].update_source = this.container_id;;

    //let data = json_data[0]["index"]  ;
    //for (var key in data) {
    //      data[key] = "-";
    //}

    let index = json_data[0]["index"]  ;
    let data = json_data[0]["type"]  ;
    for (const key of index) {
          data[key] = "-";
    }

   let fields =  { "fields": data };
   Array.prototype.splice.apply(json_data,[row_num,0].concat([fields]));
   console.log(json_data);
   console.log(json_data.length);
   //this. createResizeRowDiv();
   var xDiv = document.createElement("div");
   xDiv.className = "x_resize tb_resize";
   xDiv.setAttribute("data-resizerow", json_data.length);
   this.container.append(xDiv);
  };

  sync_next_insert_row = (row_num) => {
    let tr = this.table.insertRow(row_num + 1);
    tr.style.height = "35px";
    for (let c = 0; c < this.table_cells(); c++) {
      let newCell = tr.insertCell();
      let newText = document.createTextNode("");
      newCell.appendChild(newText);
    }
    this.table_setid();
   var xDiv = document.createElement("div");
   xDiv.className = "x_resize tb_resize";
   xDiv.setAttribute("data-resizerow", json_data.length);
   this.container.append(xDiv);
  };
  sync_prev_insert_row = (row_num) => {
    let tr = this.table.insertRow(row_num);
    tr.style.height = "35px";
    for (let c = 0; c < this.table_cells(); c++) {
      let newCell = tr.insertCell();
      let newText = document.createTextNode("");
      newCell.appendChild(newText);
    }
    this.table_setid();
   var xDiv = document.createElement("div");
   xDiv.className = "x_resize tb_resize";
   xDiv.setAttribute("data-resizerow", json_data.length);
   this.container.append(xDiv);
  };

  left_insert_col = () => {
    this.contextMenu.classList.remove("visible");
    if (this.select_td[0] == null) {
      return;
    }
    let param = this.select_td[0].id.split("_");
    let col_num = parseInt(param[2]);
    let r = 0;
    this.table.childNodes.forEach((tr) => {
      if (tr.nodeName == "TR") {
        if (r == 0) {
          var th = document.createElement("th");
          const right = this.table.querySelector("#TH_0_" + String(col_num));
          tr.insertBefore(th, right);
        } else {
          tr.insertCell(col_num);
        }
        r += 1;
      }
    });
    this.table_setid();
    this.table_th = this.table.getElementsByTagName("th");
    //setTdWidth(table);
    this.createResizeColDiv();
    this.initEvents(this.table_th);

    json_data[0]["update"] = "left-insert-col " + col_num;
    json_data[0].update_source = this.container_id;;
    //let data = json_data[0]["index"]  ;
    //for (var key in data) {
    //      data[key] = "-";
    //}
    let index = json_data[0]["index"]  ;
    let title = json_data[0]["title"]  ;
    let type  = json_data[0]["type"]  ;
    let key = "$";
    Array.prototype.splice.apply(index,[col_num,0].concat([key]));
    title[key] = key;
    type[key] = "string";
   
    let data = json_data[0]["type"]  ;
    for (const key of index) {
          data[key] = "-";
    }
  };

  right_insert_col = () => {
    this.contextMenu.classList.remove("visible");
    if (this.select_td[0] == null) {
      return;
    }
    let param = this.select_td[0].id.split("_");
    let col_num = parseInt(param[2]);
    let r = 0;
    this.table.childNodes.forEach((tr) => {
      if (tr.nodeName == "TR") {
        if (r == 0) {
          var th = document.createElement("th");
          const right = this.table.querySelector(
            "#TH_0_" + String(col_num + 1),
          );
          tr.insertBefore(th, right);
        } else {
          tr.insertCell(col_num + 1);
        }
        r += 1;
      }
    });
    this.table_setid();
    this.table_th = this.table.getElementsByTagName("th");
    //setTdWidth(table);
    this.createResizeColDiv();
    this.initEvents(this.table_th);

    json_data[0]["update"] = "right-insert-col " + col_num;
    json_data[0].update_source = this.container_id;;
    let index = json_data[0]["index"]  ;
    let title = json_data[0]["title"]  ;
    let type  = json_data[0]["type"]  ;
    let key = "X";
    Array.prototype.splice.apply(index,[col_num+1,0].concat([key]));
    title[key] = key;
    type[key] = "string";
   
    let data = json_data[0]["type"]  ;
    for (const key of index) {
          data[key] = "-";
    }
  };

  sync_left_insert_col = (col_num) => {
    let r = 0;
    this.table.childNodes.forEach((tr) => {
      if (tr.nodeName == "TR") {
        if (r == 0) {
          var th = document.createElement("th");
          const right = this.table.querySelector("#TH_0_" + String(col_num));
          tr.insertBefore(th, right);
        } else {
          tr.insertCell(col_num);
        }
        r += 1;
      }
    });
    this.table_setid();
    this.table_th = this.table.getElementsByTagName("th");
    //setTdWidth(table);
    this.createResizeColDiv();
    this.initEvents(this.table_th);
  };

  sync_right_insert_col = (col_num) => {
    let r = 0;
    this.table.childNodes.forEach((tr) => {
      if (tr.nodeName == "TR") {
        if (r == 0) {
          var th = document.createElement("th");
          const right = this.table.querySelector(
            "#TH_0_" + String(col_num + 1),
          );
          tr.insertBefore(th, right);
        } else {
          tr.insertCell(col_num + 1);
        }
        r += 1;
      }
    });
    this.table_setid();
    this.table_th = this.table.getElementsByTagName("th");
    //setTdWidth(table);
    this.createResizeColDiv();
    this.initEvents(this.table_th);

  };

  freeze_on = () => {
    //console.log("freeze on", this.freeze_decoded, this.select_td );
    if (this.select_td[0] == null) { return ;}
    let param = this.select_td[0].id.split("_");
    let row_num = parseInt(param[1]);
    let col_num = parseInt(param[2]);
    this.freeze_decoded = { c:0,r:0};
    this.table_freeze();
    this.freeze_decoded = { c:col_num,r:row_num};
    this.table_freeze();

  }
  freeze_off = () => {
    console.log("freeze off", this.freeze_decoded, this.select_td );
    this.freeze_decoded = { c:0,r:0};
    this.table_freeze();
  }
  data_dump = () => {
    this.contextMenu.classList.remove("visible");
    console.log("data_dump", this.container_id);

    let r = 0;
    this.table.childNodes.forEach((c) => {
      if (c.nodeName == "TR") {
        let id = `TR_${r}`;
        let d = 0;
        c.childNodes.forEach((c) => {
          if (c.nodeName == "TH") {
            let id = `TH_${r}_${d}`;
            console.log(id, c.innerText);
            d += 1;
          }
          if (c.nodeName == "TD") {
            let id = `TD_${r}_${d}`;
            console.log(id, c.innerText);
            d += 1;
          }
        });
        r += 1;
      }
    });
  };

  data_save = () => {
    this.contextMenu.classList.remove("visible");
    console.log("data_save", this.container_id);

    const rows = this.table.rows.length;
    const cols = this.table.rows[0].cells.length;
    const data = Array.from({ length: rows }, () => new Array(cols).fill(0));

    let tbody = this.table.childNodes[1]; //tbody
    let r = 0;
    tbody.childNodes.forEach((c) => {
      if (c.nodeName == "TR") {
        let id = `TR_${r}`;
        let d = 0;
        if (r == 0) {
          c.childNodes.forEach((c) => {
            if (c.nodeName == "TH") {
              data[r][d] = c.innerText;
              d += 1;
            }
          });
        } else {
          c.childNodes.forEach((c) => {
            if (c.nodeName == "TD") {
              data[r][d] = c.innerText;
              d += 1;
            }
          });
        }
        r += 1;
      }
    });

    console.log(data);
    var _json = JSON.stringify(data);
    console.log(_json);
  };

  menu_setup() {
    this.menu_list.forEach((item) => {
      const div = document.createElement("div");
      const text = document.createTextNode(item[0]);
      div.appendChild(text);
      div.classList.add("item");
      div.addEventListener("click", item[1]);
      this.contextMenu.appendChild(div);
    });
  }

  td_index(td) {
    let param = td.id.split("_");
    let row_num = parseInt(param[1]);
    let col_num = parseInt(param[2]);
    console.log(row_num, col_num);
  }

  json_data_save(r,c, content) {
    console.log("json_data_save");

    let index = json_data[0]["index"];
    //let index_ = $state.snapshot(index)

    //let keys = Object.keys(index_)
    //console.log("key:", keys[c]);
    //json_data[r]["fields"][keys[c]] = content;

    console.log("key:", index[c]);
    json_data[r]["fields"][index[c]] = content;
    json_data[0].update = `value-change ${r}:${c}`;
    json_data[0].update_source = this.container_id;;

    //json_data[4]["fields"]["A"] = "88888"
  }

  table_data_save(td, content) {
    console.log("table_data_save");

    let param = td.id.split("_");
    let row_num = parseInt(param[1]);
    let col_num = parseInt(param[2]);
    //console.log(this.container_id, row_num, col_num, content);
    this.table_data[row_num][col_num] = content;

    this.json_data_save(row_num, col_num, content);
  }

  table_row_insert() {

  }

  table_col_insert() {

  }

  table_setid() {
    let tbody = this.table.childNodes[1]; //tbody
    let r = 0;
    this.table.childNodes.forEach((c) => {
      if (c.nodeName == "TR") {
        c.id = `TR_${r}`;
        let d = 0;
        c.childNodes.forEach((c) => {
          if (c.nodeName == "TH") {
            c.id = `TH_${r}_${d}`;
            d += 1;
          }
          if (c.nodeName == "TD") {
            c.id = `TD_${r}_${d}`;
            d += 1;
          }
        });
        r += 1;
      }
    });
    this.tds = this.table.querySelectorAll("td");
    this.tds.forEach((td) => {
      td.removeEventListener("dblclick", null);
      td.addEventListener("dblclick", (e) => {
        if (this.editor != null) {
          let content = this.editor.getContents(true);
          this.editor.destroy();
          this.editting_td.innerHTML = content;
          this.editting_td.style.display = "";
	  this.td_index(this.editting_td);
	  this.table_data_save(this.editting_td, content);
          this.editor = null;
          this.editting_td = null;
        }
        this.editor = this.make_editor(td);
        this.editting_td = td;
      });
      td.removeEventListener("mousedown", null);
      td.addEventListener("mousedown", (e) => {
        console.log("td mousedown", e.button);
        if (this.editor != null) {
          let content = this.editor.getContents(true);
          this.editor.destroy();
          this.editting_td.innerHTML = content;
          this.editting_td.style.display = "";
	  this.td_index(this.editting_td);
	  this.table_data_save(this.editting_td, content);
          this.editor = null;
          this.editting_td = null;
        }
        if (e.button !== 0) {
          return;
        }
        if (e.shiftKey) {
          console.log("td click", "shift-key");
          if (this.select_td[1] != null) {
            this.multi_select_unset();
          }
          this.select_td[1] = td;
          this.multi_select_set();
          return;
        }
        this.td_select_drag_start = true;
        if (this.select_td[1] != null) {
          this.multi_select_unset();
        }
        if (this.select_td[0] != null) {
          this.select_td[0].classList.remove("selected");
        }
        this.select_td[0] = td;
        this.select_td[0].classList.add("selected");
      });
      td.addEventListener("mouseenter", (e) => {
        if (this.td_select_drag_start) {
          this.select_td[1] = td;
          this.multi_select_set();
        }
      });
    });
  }

  select_escape() {
      if (this.select_td[1] != null) {
          let tl = this.select_td[0] 
          this.multi_select_unset();
          this.select_td[0] = tl
          this.select_td[0].classList.add("selected");
      }
  }
  select_move(direct) {
    let td  = this.select_td[0];
      if (this.select_td[1] != null) {
          this.multi_select_unset();
      }
    let param1 = td.id.split("_");
    let r = parseInt(param1[1]);
    let c = parseInt(param1[2]);
    let row_max = json_data.length;
    let col_max = json_data[0]["index"].length;
    //console.log(r,c,row_max, col_max)

    if (direct == "down") {
        if ( r < row_max -1 ) {
              r = r + 1;
	} else {  return ;}
    } else if (direct == "up") {
        if ( r > 1 ) {
              r = r - 1;
	} else {  return ;}
    } else if (direct == "right") {
        if ( c < col_max -1 ) {
              c = c + 1;
	} else {  return ;}
    } else if (direct == "left") {
        if ( c > 0 ) {
              c = c - 1;
	} else {  return ;}
    }
    console.log(r,c,row_max, col_max)

    let tdid = "TD_" + r + "_" + c;
    let next_td = this.table.querySelector("#" + tdid);
    this.select_td[0].classList.remove("selected");
    this.select_td[0] = next_td;
    this.select_td[0].classList.add("selected");
    //this.select_td[0].scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
    this.select_td[0].scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'nearest' });
  }

  select_expand(direct) {
    let r2 = -1;
    let c2 = -1;
    let td2nd = false;
    let td  = this.select_td[0];
    if (this.select_td[1] != null) {
          //this.multi_select_unset();
          let td2  = this.select_td[1];
          let param2 = td2.id.split("_");
          r2 = parseInt(param2[1]);
          c2 = parseInt(param2[2]);
	  td2nd = true;
    }
    let param1 = td.id.split("_");
    let r = parseInt(param1[1]);
    let c = parseInt(param1[2]);
    let row_max = json_data.length;
    let col_max = json_data[0]["index"].length;

    console.log("ex1", r,c,row_max, col_max)
    if (direct == "down") {
        if ( r2 < row_max -1 ) {
              r = r + 1;
	      if(td2nd) { c = c2 ;}
	} else {  return ;}
    } else if (direct == "up") {
        if ( r2 > 1 ) {
              r = r - 1;
	      if(td2nd) { c = c2 ;}
	} else {  return ;}
    } else if (direct == "right") {
        if ( c2 < col_max -1 ) {
           //c = c + 1;
	   if(td2nd) { 
	        r = r2 ;
                c = c2 + 1;
	    } else {
                c = c + 1;
	    }
	} else {   return ;}
    } else if (direct == "left") {
        if ( c2 = -1 || c2 > 0  ) {
              //c = c - 1;
	      if(td2nd) { 
	         r = r2 ;
                 c = c2 - 1;
	      } else {
                 c = c - 1;
	      }
	} else {  return ;}
    }
    console.log("ex2", r,c,row_max, col_max)

    let tdid = "TD_" + r + "_" + c;
    let expand_td = this.table.querySelector("#" + tdid);
    //this.select_td[0].classList.remove("selected");
    //this.select_td[0] = next_td;
    //this.select_td[0].classList.add("selected");
    //this.select_td[0].scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
    //this.select_td[0].scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'nearest' });

     if (this.select_td[1] != null) {
          this.multi_select_unset();
     }
     this.select_td[1] = expand_td;
     this.multi_select_set();


  }
  select_id_gen(r1, r2, c1, c2) {
    let list = [];
    for (let r = r1; r <= r2; r++) {
      for (let c = c1; c <= c2; c++) {
        let id = "TD_" + r + "_" + c;
        list.push(id);
      }
    }
    return list;
  }

  multi_select_set() {
    let param1 = this.select_td[0].id.split("_");
    let param2 = this.select_td[1].id.split("_");
    let r1 = parseInt(param1[1]);
    let c1 = parseInt(param1[2]);
    let r2 = parseInt(param2[1]);
    let c2 = parseInt(param2[2]);
    let select_list = [];
    if (r1 < r2) {
      if (c1 < c2) {
        select_list = this.select_id_gen(r1, r2, c1, c2);
      } else if (c1 == c2) {
        select_list = this.select_id_gen(r1, r2, c1, c2);
      } else {
        select_list = this.select_id_gen(r1, r2, c2, c1);
      }
    } else if (r1 == r2) {
      if (c1 < c2) {
        select_list = this.select_id_gen(r1, r2, c1, c2);
      } else if (c1 == c2) {
      } else {
        select_list = this.select_id_gen(r1, r2, c2, c1);
      }
    } else {
      if (c1 < c2) {
        select_list = this.select_id_gen(r2, r1, c1, c2);
      } else if (c1 == c2) {
        select_list = this.select_id_gen(r2, r1, c1, c2);
      } else {
        select_list = this.select_id_gen(r2, r1, c2, c1);
      }
    }
    select_list.forEach((id) => {
      const td = this.table.querySelector("#" + id);
      td.classList.add("selected");
    });
  }

  multi_select_unset() {
    let param1 = this.select_td[0].id.split("_");
    let param2 = this.select_td[1].id.split("_");
    let r1 = parseInt(param1[1]);
    let c1 = parseInt(param1[2]);
    let r2 = parseInt(param2[1]);
    let c2 = parseInt(param2[2]);
    let select_list = [];
    if (r1 < r2) {
      if (c1 < c2) {
        select_list = this.select_id_gen(r1, r2, c1, c2);
      } else if (c1 == c2) {
        select_list = this.select_id_gen(r1, r2, c1, c2);
      } else {
        select_list = this.select_id_gen(r1, r2, c2, c1);
      }
    } else if (r1 == r2) {
      if (c1 < c2) {
        select_list = this.select_id_gen(r1, r2, c1, c2);
      } else if (c1 == c2) {
      } else {
        select_list = this.select_id_gen(r1, r2, c2, c1);
      }
    } else {
      if (c1 < c2) {
        select_list = this.select_id_gen(r2, r1, c1, c2);
      } else if (c1 == c2) {
        select_list = this.select_id_gen(r2, r1, c1, c2);
      } else {
        select_list = this.select_id_gen(r2, r1, c2, c1);
      }
    }
    select_list.forEach((id) => {
      const td = this.table.querySelector("#" + id);
      td.classList.remove("selected");
    });
    this.select_td[1] = null;
  }

  table_dump() {
    console.log("table rows len", this.table.rows.length);
    console.log("table cells len", this.table.rows[0].cells.length);

    let tbody = this.table.childNodes[1]; //tbody
    let r = 0;
    tbody.childNodes.forEach((c) => {
      if (c.nodeName == "TR") {
        console.log(c.nodeName);
        let d = 0;
        c.childNodes.forEach((c) => {
          if (c.nodeName == "TH") {
            console.log(" ", c.nodeName, r, d);
            d += 1;
          }
          if (c.nodeName == "TD") {
            console.log(" ", c.nodeName, r, d);
            d += 1;
          }
        });
        r += 1;
      }
    });
  }

  //=======================================================================
  mouseDown_y = (e) => {
    //this.resize = this;
    this.resize = e.target;
    this.resizeCheck = this.resize.classList.contains("y_resize");
    var col_index = parseInt(this.resize.getAttribute("data-resizecol")) - 1;
    this.col_element = this.table_th[col_index];
    this.next_element = this.table_th[col_index + 1];
    this.y_dragStart = true;
    this.y_cursorStart = this.resizeCheck ? event.pageX : event.pageY;
    var elm_bound = this.col_element.getBoundingClientRect();
    this.width = elm_bound.width;
    this.table_wt = this.table.offsetWidth;
    if (this.next_element != undefined) {
      var next_bound = this.next_element.getBoundingClientRect();
      this.next_width = next_bound.width;
    }
    //this.resize_left = this.getBoundingClientRect().left - bodyRect.left;
    this.resize_left =
      //this.resize.getBoundingClientRect().left - this.bodyRect.left - this.tableRect.left;
      this.resize.getBoundingClientRect().left - this.tableRect.left;
  };

  mouseMove_y = () => {
    if (this.y_dragStart) {
      var cursorPosition = this.resizeCheck ? event.pageX : event.pageY;
      var mouseMoved = cursorPosition - this.y_cursorStart;
      var newLeft = this.resize_left + mouseMoved;
      var newWidth = this.width + mouseMoved;
      var new_nextWidth, new_nextHeight;
      if (this.next_element != undefined) {
        new_nextWidth = this.next_width - mouseMoved;
      }
      if (
        newWidth > 30 &&
        (new_nextWidth > 30 || this.next_element == undefined)
      ) {
        this.col_element.style.cssText = "width: " + newWidth + "px;";
        if (this.next_element != undefined) {
          this.next_element.style.cssText = "width: " + new_nextWidth + "px";
        } else {
          this.table.style.width = this.table_wt + mouseMoved + "px";
        }
        this.resize.style.cssText = "left: " + newLeft + "px;";
      }
    }
  };

  mouseUp_y = () => {
    if (this.y_dragStart) {
      this.y_dragStart = false;
      this.table_freeze();
    }
    if (this.td_select_drag_start) {
      this.td_select_drag_start = false;
    }
  };

  //=======================================================================

  mouseDown_x = (e) => {
    this.resize = e.target;
    this.resizeCheck = this.resize.classList.contains("x_resize");
    var row_index = parseInt(this.resize.getAttribute("data-resizerow")) - 1;
    this.row_element = this.table_tr[row_index];
    this.next_row_element = this.table_tr[row_index + 1];
    this.x_dragStart = true;
    this.x_cursorStart = this.resizeCheck ? event.pageY : event.pageX;
    var elm_bound = this.row_element.getBoundingClientRect();
    this.height = elm_bound.height;
    this.table_wt = this.table.offsetWidth;
    this.table_ht = this.table.offsetHight;
    this.resize_top =
      this.resize.getBoundingClientRect().top - this.tableRect.top;
  };

  mouseMove_x = () => {
    if (this.x_dragStart) {
      //var cursorPosition = this.resizeCheck ? event.pageX : event.pageY;
      var cursorPosition = this.resizeCheck ? event.pageY : event.pageX;
      var mouseMoved = cursorPosition - this.x_cursorStart;

      var newTop = this.resize_top + mouseMoved;
      var newHeight = this.height + mouseMoved;

      var new_nextWidth, new_nextHeight;
      this.row_element.style.cssText = "height: " + newHeight + "px;";
      if (this.next_row_element != undefined) {
        // this.next_row_element.style.cssText = "height: " + new_nextHeight + "px";
      } else {
        this.table.style.hight = this.table_wt + mouseMoved + "px";
      }
      this.resize.style.cssText = "top: " + newTop + "px;";
    }
  };

  mouseUp_x = () => {
    if (this.x_dragStart) {
      this.x_dragStart = false;
      this.resetResizeRowDiv();
      this.table_freeze();
    }
    if (this.td_select_drag_start) {
      this.td_select_drag_start = false;
    }
  };

  //=======================================================================

  initEvents(table_th) {
   //console.log("initEvent");
    document.body.addEventListener("mousemove", this.mouseMove_y);
    document.body.addEventListener("mouseup", this.mouseUp_y);
    var y_resize = this.container.getElementsByClassName("y_resize");

    var th_length = this.table_th.length;
    for (var i = 0; i < th_length; i++) {
      y_resize[i].addEventListener("mousedown", this.mouseDown_y);
      this.table_th[i].style.width = this.th_width + "px";
    }

    document.body.addEventListener("mousemove", this.mouseMove_x);
    document.body.addEventListener("mouseup", this.mouseUp_x);
    var x_resize = this.container.getElementsByClassName("x_resize");
    for (let i = 0; i < x_resize.length; i++) {
      x_resize[i].addEventListener("mousedown", this.mouseDown_x);
    }
  }

  setTdWidth = () => {
    var elm_bound = this.table.getBoundingClientRect();
    var table_wt = elm_bound.width;
    var th_length = this.table_th.length;
    this.th_width = table_wt / th_length;
  };

  createResizeColDiv() {
    const resizes = this.container.querySelectorAll(".y_resize");
    resizes.forEach((ele) => {
      ele.remove();
    });
    var th_length = this.table_th.length;
    for (var i = 1; i <= th_length; i++) {
      var yDiv = document.createElement("div");
      yDiv.className = "y_resize tb_resize";
      yDiv.setAttribute("data-resizecol", i);
      var leftPos = i * this.th_width + 0.5;
      yDiv.style.cssText = "left: " + leftPos + "px;";
      this.container.append(yDiv);
    }
  }
  createResizeRowDiv() {
    const resizes = this.container.querySelectorAll(".x_resize");
    resizes.forEach((ele) => {
      ele.remove();
    });
    let i = 1;
    this.table.childNodes.forEach((c) => {
      if (c.nodeName == "TR") {
        var xDiv = document.createElement("div");
        xDiv.className = "x_resize tb_resize";
        xDiv.setAttribute("data-resizerow", i);
        //var topPos = i * this.th_width + 0.5;
        var topPos = c.offsetTop + c.offsetHeight - 4;
        xDiv.style.cssText = "top: " + topPos + "px;";
        this.container.append(xDiv);
        i += 1;
      }
    });
  }

  resetResizeRowDiv() {
    function getTarget(list, num) {
      for (let i = 0; i < list.length; i++) {
        var row_index = parseInt(list[i].getAttribute("data-resizerow"));
        if (row_index == num) {
          return list[i];
        }
      }
      return null;
    }

    var x_resize = this.container.getElementsByClassName("x_resize");
    let i = 1;
    this.table.childNodes.forEach((c) => {
      if (c.nodeName == "TR") {
        var topPos = c.offsetTop + c.offsetHeight - 4;
        var target = getTarget(x_resize, i);
        //console.log(i, target);
        target.style.cssText = "top: " + topPos + "px;";
        i += 1;
      }
    });
  }

  make_editor(ele) {
    let editor = suneditor.create(ele, {
      plugins: plugins,
      mode: "balloon",
      //  ('classic', 'inline', 'balloon', 'balloon-always'). default: 'classic' {String}

      buttonList: [
        //['undo', 'redo'],
        //['font', 'fontSize', 'formatBlock'],
        ["fontSize"],
        //['paragraphStyle', 'blockquote'],
        //['bold', 'underline', 'italic', 'strike', 'subscript', 'superscript'],
        ["bold", "underline", "italic"],
        ["fontColor", "hiliteColor", "textStyle"],
        //['removeFormat'],
        //'/', // Line break
        //['outdent', 'indent'],
        ["align", "horizontalRule", "list", "lineHeight"],
        //['table', 'link', 'image', 'video', 'audio' ], // You must add the 'katex' library at options to use the 'math' plugin.
        //['fullScreen', 'showBlocks', 'codeView'],
        //['preview', 'print'],
        //['save', 'template'],
      ],
      resizingBar: false,
      showPathLabel: false,
      resizeEnable: false,
    });
    return editor;
  }

  sum_top_height(r) {
    let height = 0;
    const offset = 0;
    for (let i = 0; i < r; i++) {
      let tdid;
      if (i == 0) {
        tdid = "TH_" + String(i) + "_" + "0";
      } else {
        tdid = "TD_" + String(i) + "_" + "0";
      }
      let ele = this.table.querySelector("#" + tdid);
      if (ele == null) {
        return 0;
      }
      height += ele.clientHeight;
    }
    return height + offset;
  }

  sum_left_width(c) {
    let width = 0;
    const offset = 0;
    for (let i = 0; i < c; i++) {
      let tdid = "TH_" + "0" + "_" + String(i);
      let ele = this.table.querySelector("#" + tdid);
      if (ele == null) {
        return 0;
      }
      width += ele.clientWidth;
    }
    return width + offset;
  }

  isrFreeze() {
    return this.freeze_decoded.c > 0;
  }

  unFreeze() {
    console.log("unFreeze");
    this.freeze_decoded = { c: 0, r: 0 };
    this.table_freeze();
  }

  onFreeze() {
    console.log("onFreeze");
    this.freeze_decoded = { c: 3, r: 3 };
    this.table_freeze();
  }

  freezeStyle_tr_set(tr, r) {
    if (this.isrFreeze()) {
      if (r < this.freeze_decoded.r) {
        let tophight = this.sum_top_height(r);
        tr.style.position = "sticky";
        tr.style.top = `${tophight}px`;
        tr.style.background = "white";
        tr.style.zIndex = 105;
      }
      if (r == this.freeze_decoded.r - 1) {
        tr.style.borderBottom = "2px solid green";
      }
    } else {
      tr.style.position = "";
      tr.style.top = "";
      tr.style.background = "";
      tr.style.zIndex = 0;
      tr.style.borderBottom = "";
    }
  }

  freezeStyle_td_set(td, c, r) {
    if (this.isrFreeze()) {
      if (c < this.freeze_decoded.c) {
        let leftwidth = this.sum_left_width(c);
        td.style.position = "sticky";
        td.style.left = `${leftwidth}px`;
        td.style.background = "white";
        td.style.zIndex = 100;
        if (c == this.freeze_decoded.c - 1) {
          td.style.borderRight = "2px solid green";
        }
      }
      if (r == this.freeze_decoded.r - 1) {
        td.style.borderBottom = "2px solid green";
      }
    } else {
      td.style.position = "";
      td.style.left = "";
      td.style.background = "";
      td.style.zIndex = 0;
      td.style.borderRight = "";
      td.style.borderBottom = "";
    }
  }

  freezeStyle_tr(r) {
    let style = "";
    if (this.isrFreeze()) {
      if (r < this.freeze_decoded.r) {
        let tophight = this.sum_top_height(r);
        style = `position: -webkit-sticky; position: sticky;  top: ${tophight}px;  background: white; z-index: 105; `;
      }
      if (r == this.freeze_decoded.r - 1) {
        style += "border-bottom: 2px solid green; ";
      }
    }
    return style;
  }

  freezeStyle_td(c, r) {
    let style = "";
    if (this.isrFreeze()) {
      if (c < this.freeze_decoded.c) {
        let leftwidth = this.sum_left_width(c);
        //style += `position: -webkit-sticky; position: sticky; left: ${leftwidth}px; width: 100px;  min-width: 100px;  background: white; z-index:100; `
        style += `position: -webkit-sticky; position: sticky; left: ${leftwidth}px;   background: white; z-index:100; `;
        if (c == this.freeze_decoded.c - 1) {
          style += "border-right: 2px solid green; ";
        }
      }
      if (r == this.freeze_decoded.r - 1) {
        style += "border-bottom: 2px solid green; ";
      }
    }
    return style;
  }
} // class end

let { 
        //json_data = {},
        json_data =  $bindable({}),
        container_id,
	table_width = "100%",
	scrollable = false,
        //table = $bindable("") 

} = $props();

let table = new Table(container_id, table_width);

export function get_table() {
   return table;
   }

let init = true;

$effect(() => {
 if (init) {
   console.log(table.container_id, "init....");
   table.init(json_data);
   table.table_freeze();
   init = false;
 } else {
   if (json_data[0].update_source != container_id) {
     console.log(table.container_id, "update....", json_data[0].update);
     console.log(json_data);
     let param = json_data[0].update.split(" ");
     let update_type = param[0];
     if (update_type == "value-change") {
         console.log("table.update");
        table.update(json_data);
     } else if (update_type == "prev-insert-row") {
        console.log("prev");
        let row_num = parseInt(param[1]);
        table.sync_prev_insert_row(row_num);
     } else if (update_type == "next-insert-row") {
        console.log("next");
        let row_num = parseInt(param[1]);
        table.sync_next_insert_row(row_num);
     } else if (update_type == "left-insert-col") {
        console.log("left");
        let col_num = parseInt(param[1]);
        table.sync_left_insert_col(col_num);
     } else if (update_type == "right-insert-col") {
        console.log("right");
        let col_num = parseInt(param[1]);
        table.sync_right_insert_col(col_num);
     }
   }
 }

 });


</script>


<main  class="container {scrollable ? 'container-scrollable' : ''}" id="{container_id}" bind:this={table.container}>

</main>

<style>

</style>
