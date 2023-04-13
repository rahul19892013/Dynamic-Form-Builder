
<template>
  <div id="items">
  Design Your Form<br/>
  <table id="employees">
    <th>
      Command Pallate
    </th>
    <th style="width:60%;">
     Workflow
    </th>
    <th style="width:20%;" v-if="dropedElement">
    Properties
    </th>
    <tr>
      <td>
        <label draggable="true" @dragstart="dragStart($event)" id="label1">Label / Caption</label><br>
      </td>
      
      <td rowspan="5">
        <table style="width:100%" id="tblFormBuilder">
          <tr>
          <td>
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="Drag and Drop Fields Here">
       
      </div>
    </td>
    <td>
      
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="Drag and Drop Fields Here">
      
      </div>
    </td>
  </tr>
    </table>
      </td>
      <td v-if="dropedElement">
        <input type="text" id="cntrlFieldCaptionValue" placeholder="Caption"/>
      </td>
     
    </tr>
    <tr>
      <td>
        <input draggable="true" @dragstart="dragStart($event)" id="textBox1" type="text" placeholder="textbox" />
      </td>
      <td v-if="dropedElement">
      <input type="text" id="cntrlFieldDescriptionValue" placeholder="Field Description"/>
      </td>
    </tr>
    <tr>
      <td>
        <input draggable="true" @dragstart="dragStart($event)" id="checkBox1" type="checkbox" value="CheckBox"/>&nbsp;
        <label draggable="true" @dragstart="dragStart($event)" id="checkBox1">Checkbox</label><br>
      </td>
      <td v-if="dropedElement">
        <input type="text" id="cntrltextMinValue" placeholder="Minimum Value"/>
      </td>
    </tr>
    <tr>
      <td>
        <label>Options</label><br>
        <select draggable="true" @dragstart="dragStart($event)" id="select1" style="width:85%;">
         
        </select>
      </td>
      <td v-if="dropedElement">
        <input type="text" id="cntrltextMaxValue" placeholder="Maximum Value"/>
        <input type="checkbox" id="cntrlChkValue" value=""/> Required
      </td>
    </tr>
    <tr>
      <td>
        <input draggable="true" @dragstart="dragStart($event)" id="button1" type="submit" value="Submit" />
      </td>
      <td v-if="dropedElement">
        <label>Data Type</label><br>
        <select id="ddlDataType">
        <option value="1">Int</option>
        <option value="2">Text</option>
        <option value="3">Decimal</option>
        <option value="4">Boolean</option>
  </select>
      </td>
    </tr>
    <tr>
      <td colspan="2">
        <input type="submit" @click="saveSchema" value="Save Schema" style="width: 20%; float: right;"/>
      </td>
      <td v-if="dropedElement && isDropdown">
        <input type="submit" value="Add Lookup" style="width: auto;"/>
      </td>
    </tr>
  </table>
  </div>
  </template>
  
  <script>
export default {
  name: 'App',
  components:{
     
  },
  data(){
    return{
      num:0,
      evenOrOddMessage:"Initial",
      formValues:{
        firstName:"",
        lastName:"",
        gender:"",
        experience:"0 yrs",
        skills:[]
      },
      
      dropedElement:false,
      isDropdown:false
    }
  },
  methods:{

    dragStart(event){
      event.dataTransfer.dropEffect='move';
      event.dataTransfer.effectAllowed='move';
      event.dataTransfer.setData('itemID',event.target.id)
    },

    drop(event){
      this.isDropdown=false;
      const control=document.getElementById(event.dataTransfer.getData('itemID'));
      switch(control.id.toUpperCase())
      {
        case 'LABEL1':{
                    const labelCntrl=document.createElement("label");
                    labelCntrl.style.display='block';
                    labelCntrl.innerHTML='Label Added';
                    labelCntrl.contentEditable="true";
                    event.target.appendChild(labelCntrl);
        }
          break;
          case 'TEXTBOX1':{
                    const inputCntrl=document.createElement('input');
                    inputCntrl.style.display='block';
                    inputCntrl.type = 'text';
                    inputCntrl.placeholder='Enter Value';
                    event.target.appendChild(inputCntrl);
          }
          break;
          case 'CHECKBOX1':{
                    const checkboxCntrl=document.createElement('input');
                    checkboxCntrl.style.display='block';
                    checkboxCntrl.type = 'checkbox';
                    checkboxCntrl.innerHTML='checkBox';
                    event.target.appendChild(checkboxCntrl);
          }
          break;
          case 'BUTTON1':{
                    const btnSubmitCntrl=document.createElement('input');
                    btnSubmitCntrl.style.display='block';
                    btnSubmitCntrl.type = 'submit';
                    event.target.appendChild(btnSubmitCntrl);
                    this.isDropdown=true;
          }
            break;
          default:
                console.log('No Control Found');
            break;
      }
      this.dropedElement=true;
    }
  },
}
</script>

<style>

input[type=text], select {
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type=submit] {
  width: 40%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type=submit]:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}

#employees {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 80%;
}

#employees td, #employees th {
  border: 1px solid #ddd;
  padding: 8px;
}

#employees tr:nth-child(even){background-color: #f2f2f2;}

#employees tr:hover {background-color: #ddd;}

#employees th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04AA6D;
  color: white;
}

.darkButton {background-color: #555555;color: #f2f2f2; border-radius: 10px;}

[contentEditable=false]:empty:not(:focus):before{
        content:attr(data-text)
    }
</style>
