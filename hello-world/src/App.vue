
<template>
  <br/>
  &nbsp;&nbsp;<button class="button darkButton" type="submit" @click="GetItems">Get Items</button>&nbsp;
  <button class="button darkButton" type="submit" @click="GetActions">Get Actions</button>&nbsp;
  <button class="button darkButton" type="submit" @click="GetLookups">Get Lookups</button>&nbsp;
  <button class="button darkButton" type="submit" @click="GetFields">Get Fields</button>&nbsp;
  <button class="button darkButton" type="submit" style="float: right;" @click="GetDesignForm">Design Form</button>
 
  <div id="items" v-if="viewItems">
    <br/><br/>
  <table id="employees">
    <th>
      Sr No.
    </th>
    <th>
       Name
    </th>
    <th>
      Description
    </th>
    <th>
       Active
    </th>
    <th>
      Created Date
    </th>
    <tr v-for="(item,index) in items" :key="index">
      <td>{{ index+1 }}</td>
      <td>{{ item.name }}</td>
      <td>{{ item.description }}</td>
      <td>{{ item.isActive }}</td>
      <td>{{ formatDate(item.created)}}</td>
    </tr>
  </table>
  </div>

  <div id="actions" v-if="viewActions">
    <br/><br/>
  <table id="employees">
    <th>
      Sr No.
    </th>
    <th>
       ItemId
    </th>
    <th>
       ActionId
    </th>
    <th>
       Name
    </th>
    <th>
      Description
    </th>
    <th>
       Active
    </th>
    <th>
      Created Date
    </th>
    <tr v-for="(action,index) in actions" :key="index" @click="rowClick(action.itemId,action.id)" style="cursor: pointer;">
      <td>{{ index+1 }}</td>
      <td>{{ action.itemId }}</td>
      <td>{{ action.id }}</td>
      <td>{{ action.name }}</td>
      <td>{{ action.description }}</td>
      <td>{{ action.isActive }}</td>
      <td>{{ formatDate(action.created) }}</td>
    </tr>
  </table>
  </div>

  <div id="lookups" v-if="viewLookups">
    <br/><br/>
  <table id="employees">
    <th>
      Sr No.
    </th>
    <th>
       ItemId
    </th>
    <th>
       Name
    </th>
    <th>
      Description
    </th>
    <th>
      Value
    </th>
    <th>
      Id Value
    </th>
    <th>
      Item Id
    </th>
    <th>
       Active
    </th>
    <th>
      Created Date
    </th>
    <tr v-for="(lookup,index) in lookups" :key="index">
      <td>{{ index+1 }}</td>
      <td>{{ lookup.itemId }}</td>
      <td>{{ lookup.name }}</td>
      <td>{{ lookup.description }}</td>
      <td>{{ lookup.value }}</td>
      <td>{{ lookup.idValue }}</td>
      <td>{{ lookup.itemId }}</td>
      <td>{{ lookup.isActive }}</td>
      <td>{{ formatDate(lookup.created) }}</td>
    </tr>
  </table>
  </div>

  <div id="lookups" v-if="viewFields">
    <br/><br/>
  <table id="employees">
    <th>
      Sr No.
    </th>
    <th>
      Caption
    </th>
    <th>
      Description
    </th>
    <th>
      Data Type
    </th>
    <th>
      Field Type
    </th>
    <th>
      Is Required
    </th>
    <th>
      Sequence
    </th>
    <th>
      Min Length
    </th>
    <th>
      Max Length
    </th>
    <th>
      LookUp Id
    </th>
    <th>
      Item Id
    </th>
    <th>
      Action Id
    </th>
    <th>
       Active
    </th>
    <th>
      Created Date
    </th>
    <tr v-for="(field,index) in fields" :key="index">
      <td>{{ index+1 }}</td>
      <td>{{ field.caption }}</td>
      <td>{{ field.description }}</td>
      <td>{{ field.dataType }}</td>
      <td>{{ field.fieldType }}</td>
      <td>{{ field.isRequired }}</td>
      <td>{{ field.minLength }}</td>
      <td>{{ field.maxLength }}</td>
      <td>{{ field.sequence }}</td>
      <td>{{ field.lookUpId }}</td>
      <td>{{ field.itemId }}</td>
      <td>{{ field.actionId }}</td>
      <td>{{ field.isActive }}</td>
      <td>{{ formatDate(field.created) }}</td>
    </tr>
  </table>
  </div>

  <div id="designForm" v-if="viewDesignForm"><br/>
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
        <input class="grabbable" draggable="true" @dragstart="dragStart($event)" id="button1" type="submit" value="Submit" />
      </td>
      
      <td>
        <table style="width:100%" id="tblFormBuilder">
          <tr>
          <td>
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="1">
          
      </div>
    </td>
    <td>
      
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="2">
      
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
        <input class="grabbable" draggable="true" @dragstart="dragStart($event)" id="textBox1" type="text" placeholder="textbox" />
      </td>


      <td>
        <table style="width:100%" id="tblFormBuilder">
          <tr>
          <td>
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="3">
       
      </div>
    </td>
    <td>
      
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="4">
      
      </div>
    </td>
  </tr>
    </table>
      </td>


      <td v-if="dropedElement">
      <input type="text" id="cntrlFieldDescriptionValue" placeholder="Field Description"/>
      </td>
    </tr>
    <tr>
      <td>
        <input class="grabbable" draggable="true" @dragstart="dragStart($event)" id="checkBox1" type="checkbox" value="CheckBox"/>&nbsp;
        <label class="grabbable" draggable="true" @dragstart="dragStart($event)" id="checkBox1">Checkbox</label><br>
      </td>
      <td>
        <table style="width:100%" id="tblFormBuilder">
          <tr>
          <td>
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="5">
       
      </div>
    </td>
    <td>
      
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="6">
      
      </div>
    </td>
  </tr>
    </table>
      </td>
      <td v-if="dropedElement">
        <input type="text" id="cntrltextMinValue" placeholder="Minimum Length"/>
      </td>
    </tr>
    <tr>
      <td>
        <label>Options</label><br>
        <select class="grabbable" draggable="true" @dragstart="dragStart($event)" id="select1" style="width:30%;">
         
        </select>
      </td>
      <td>
        <table style="width:100%" id="tblFormBuilder">
          <tr>
          <td>
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="7">
       
      </div>
    </td>
    <td>
      
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="8">
      
      </div>
    </td>
  </tr>
    </table>
      </td>
      <td v-if="dropedElement">
        <input type="text" id="cntrltextMaxValue" placeholder="Maximum Length"/>
      </td>
    </tr>
    <tr>
      <td>
        <label class="grabbable" draggable="true" @dragstart="dragStart($event)" id="label1">Label / Caption</label><br>
      </td>
      <td>
        <table style="width:100%" id="tblFormBuilder">
          <tr>
          <td>
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="9">
       
      </div>
    </td>
    <td>
      
        <div @drop="drop($event)" @dragenter.prevent  @dragover.prevent contentEditable=false data-text="10">
      
      </div>
    </td>
  </tr>
    </table>
      </td>
      <td v-if="dropedElement">
        <label>Data Type</label>&nbsp;
        <select id="ddlDataType">
        <option value="1">Int</option>
        <option value="2">Text</option>
        <option value="3">Decimal</option>
        <option value="4">Boolean</option>
  </select> &nbsp;
  <input type="checkbox" id="cntrlChkValue" value=""/> Required
      </td>
    </tr>
    <tr>
      <td colspan="2">
        <input type="submit" @click="saveSchema" value="Save Schema" style="float: right;" class="button"/>
      </td>
      <td v-if="dropedElement">
        <input v-if="isDropdown" type="submit" value="Add Lookup" style="width: auto;"/>&nbsp;
        <label v-if="showSequence">Sequence</label>&nbsp;
        <select v-if="showSequence" id="ddlSequence">
        <option v-for="(seq,index) in sequence" :key="index" value="index">
          {{ seq }}
          </option>
        </select>
      </td>
    </tr>
  </table>
</div>

<div id="renderQuasarForm" v-if="renderForm" class="q-pa-md" style="max-width: 50%">
  <label class="container">Display Quasar Form<input type="checkbox" name="checkbox" value="1" @change="displayQuasarForm">
    <span class="checkmark"></span>
    </label>
    <hr/>
  <br/>  
  <div id="qForm">
  <div class="q-pa-md" style="max-width: 400px" v-if="quasarForm">
  <q-form ref="formComponent" action="" method="" class="q-gutter-md">
    <div v-for="(renderFormField,index) in renderFormFields" :key="index">
    {{ renderFormField.caption }}
    {{ renderFormField.isRequired }}
    <q-input v-model="inputdata" :maxlength="renderFormField.maxLength" :name="renderFormField.fieldType" :placeholder="renderFormField.caption" v-if="renderFormField.fieldType=='TextBox'" :rules="[val=> val && val.length>0||renderFormField.caption]"/>
    <q-select filled v-model="inputdata" :options="options" label="Standard" :options-html="optionsHtml" :name="renderFormField.fieldType" v-if="renderFormField.fieldType=='Dropdown'"/>
  </div>
  <q-btn type="submit" label="Submit"/>
  </q-form>
</div>
</div>
<div id="htmlForm" v-if="!quasarForm">
  <form action="" method="" v-for="(renderFormField,index) in renderFormFields" :key="index">
    {{ renderFormField.caption }}
    <input v-model="inputdata" name="renderFormField.fieldType" placeholder="sasas" v-if="renderFormField.fieldType=='TextBox'"/>
    <select filled v-model="inputdata" name="renderFormField.fieldType" v-if="renderFormField.fieldType=='Dropdown'"/>
  </form>
</div>
</div>

<center>
<div id="error" class="error" v-if="isError">
  {{ errorMessage }}
</div>
</center>

</template>
  
<script>
import axios from 'axios'
import { ref } from 'vue'
import { APISettings } from './ApiSettings.js';

const formComponent=ref();
export default {
  name: 'App',
  components:{},
  data(){
    return{
      sequence:[],
      controlCount:0,
      evenOrOddMessage:"Initial",
      formValues:{
        firstName:"",
        lastName:"",
        gender:"",
        experience:"0 yrs",
        skills:[]
      },
      items:{},
      actions:{},
      fields:{},
      lookups:{},
      renderFormFields:{},
      selectedItemId:0,
      selectedActionId:0,
      dropedElement:false,
      isDropdown:false,
      pageSize:25,
      optionsHtml: ref(true),
      options: [{}],
      viewItems:false,
      viewActions:false,
      viewDesignForm:false,
      viewLookups:false,
      viewFields:false,
      renderForm:false,
      quasarForm:false,
      isError:false,
      errorMessage:'',
      showSequence:false,
    }
  },
  methods:{
    GetItems(){
      this.resetViews();
      this.viewItems=true;
      this.fields={};
      this.actions={};
      axios.get(APISettings.baseURL +'Items/GetItems',
      { 
        headers: {
        'Authorization':APISettings.token
      },
        params: this.axiosParams()
    }).then(res=>{
      this.items=res.data.data.items
      console.log(res)
    }).catch(e=>{
      this.showError(e);
    })
    },

    GetActions(){
      this.resetViews();
      this.viewActions=true;
      this.fields={};
      this.actions={};
      axios.get(APISettings.baseURL +'Actions/GetActions',
      { 
        headers: {
          'Authorization':APISettings.token
      },
        params: this.axiosParams()
    }).then(res=>{
      this.actions=res.data.data.items
      console.log(res.data.data.items)
    }).catch(e=>{
      this.showError(e);
    })
    },

    GetLookups(){
      this.resetViews();
      this.viewLookups=true;
      this.lookups={};
      axios.get(APISettings.baseURL +'Lookups/GetLookups',
      { 
        headers: {
          'Authorization':APISettings.token
      },
        params: this.axiosParams()
    }).then(res=>{
      this.lookups=res.data.data.items
      console.log(res.data.data.items)
    }).catch(e=>{
      this.showError(e);
    })
    },
    
    GetFields(){
      this.resetViews();
      this.viewFields=true;
      this.fields={};
      axios.get(APISettings.baseURL +'Fields/GetFields',
      { 
        headers: {
          'Authorization':APISettings.token
      },
      params: this.axiosParams(1,1)
    }).then(res=>{
      this.fields=res.data.data.items
      console.log(res.data.data.items);
    }).catch(e=>{
      this.showError(e);
    })
    },

    axiosParams(itemId=0,actionId=0) {
        const params = new URLSearchParams();
        params.append('PageNumber', '1');
        params.append('PageSize', this.pageSize);
        params.append('ItemId',itemId);
        params.append('ActionId',actionId);
        return params;
    },

    submitForm(){
    alert('ER submitted');
    },

    saveSchema(){
      const article = { 'caption': 'Test',
          'description': 'Test',
          'dataType': 'Int',
          'fieldType': 'TextBox',
          'minLength': 0,
          'maxLength': 0,
          'lookUpId': 0,
          'itemId': 0,
          'actionId': 0,
          'isActive': true };
      axios.post(APISettings.baseURL +'Fields/AddField',
      article,
      { 
        headers: {
          'Authorization':APISettings.token
      },
    }).then(res=>{
      console.log(res.data.data.items)
    }).catch(e=>{
      this.showError(e);
    })
    },

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
                    this.controlCount+=1;
        }
          break;
          case 'TEXTBOX1':{
                    const inputCntrl=document.createElement('input');
                    const deleteCntrlElement=document.createElement('i');
                    inputCntrl.type = 'text';
                    inputCntrl.placeholder='Enter Value';
                    event.target.onmouseover=inputCntrl.onmouseover=function(e){
                      deleteCntrlElement.style.display='inline-block';
                    }

                    event.target.onmouseout=inputCntrl.onmouseout=function(e){
                      deleteCntrlElement.style.display='none';
                    }

                    deleteCntrlElement.style.display='none';
                    deleteCntrlElement.className='fa fa-trash deleteIconcss';
                    deleteCntrlElement.onclick=function(e){
                      alert(e.target);
                      console.log(e.target.parentNode.remove());
                    }
                    event.target.appendChild(inputCntrl);
                    event.target.appendChild(document.createTextNode(" "));
                    event.target.appendChild(deleteCntrlElement);
                    this.controlCount+=1;
          }
          break;
          case 'CHECKBOX1':{
                    const checkboxCntrl=document.createElement('input');
                    checkboxCntrl.style.display='block';
                    checkboxCntrl.type = 'checkbox';
                    checkboxCntrl.innerHTML='checkBox';
                    event.target.appendChild(checkboxCntrl);
                    this.controlCount+=1;
          }
          break;
          case 'BUTTON1':{
                    const btnSubmitCntrl=document.createElement('input');
                    btnSubmitCntrl.style.display='block';
                    btnSubmitCntrl.type = 'submit';
                    event.target.appendChild(btnSubmitCntrl);
                    this.isDropdown=true;
                    this.controlCount+=1;
          }
            break;
          default:
                console.log('No Control Found');
            break;
      }
      this.dropedElement=true;
      this.rebindSequence(this.controlCount);
      if(this.controlCount>1){
        this.showSequence=true;
      }
    },
    
    GetDesignForm(){
      this.resetViews();
      this.viewDesignForm=true;
    },

    resetViews(){
      this.items={};
      this.fields={};
      this.lookups={};
      this.actions={};
      this.isError=false;
      this.errorMessage='';
      this.viewItems=false;
      this.renderForm=false;
      this.viewFields=false;
      this.viewActions=false;
      this.viewLookups=false;
      this.renderFormFields={};
      this.viewDesignForm=false;
      this.sequence.splice(0, this.controlCount);
      this.controlCount=0;
      this.showSequence=false;
      this.dropedElement=false;
    },

    rowClick(itemId,actionId){
      this.resetViews();
      this.renderFormFields={};
      this.renderForm=true;
      axios.get(APISettings.baseURL +'Fields/GetFields',
      { 
        headers: {
          'Authorization':APISettings.token
      },
      params: this.axiosParams(itemId,actionId)
    }).then(res=>{
      this.renderFormFields=res.data.data.items
      this.createDynamicForm(res.data.data.items);
    })
    .catch(e=>{
      this.showError(e);
    })

    //gettingLookups if any
    axios.get(APISettings.baseURL +'Lookups/GetLookups',
      { 
        headers: {
          'Authorization':APISettings.token
      },
        params: this.axiosParams(itemId,actionId)
    }).then(res=>{
     
      res.data.data.items.forEach(element => {
       
        this.options.push([{
        label: element.name,
          value:  element.idValue
      }])
     
      });
    }).catch(e=>{
      this.showError(e);
    })
    },

    createDynamicForm(items){

      //Ignore this is just for testing and it isn't completed
      const cntrl=document.getElementById('qForm');
      
      items.forEach(element => {
      console.log(element.fieldType)
      switch(element.fieldType.toUpperCase()){
        case 'TEXTBOX':{
          const inputCntrl=document.createElement('q-input');
          inputCntrl.type = 'text';
          inputCntrl.setAttribute(':maxlength',element.maxLength);
          inputCntrl.setAttribute(':name',element.fieldType);
          inputCntrl.setAttribute(':placeholder',element.fieldType.caption);
          inputCntrl.setAttribute(':rules',element.maxLength);
          cntrl.appendChild(inputCntrl);
        }
        break;
        default:
          alert(element.fieldType.toUpperCase() + ' control could not be rendered');
          break;
      }
      }); 
    },

    localizeDate(date) {
      // Date picker uses ISO format (yyyy-mm-dd), which is UTC. The data
      // contains locale specific date strings (mm/dd/yyyy), which `Date`
      // parses with local time-zone offset instead of UTC. Normalize the
      // ISO date so we're comparing local times.
      if (!date || !date.includes('-')) return date
      const [yyyy, mm, dd] = date.split('-')
      return new Date(`${mm}/${dd}/${yyyy}`)
    },

    formatDate(date) {
      return new Intl.DateTimeFormat('en-US', { dateStyle: 'long' }).format(new Date(date))
    },

    displayQuasarForm(e){
      if(e.target.checked)
        this.quasarForm=true;
      else
        this.quasarForm=false;
    },

    showError(e){
      this.isError=true;
     if(typeof(e.response)!='undefined'){
      switch(e.response.status){
        case 401:{
        this.errorMessage='Unauthorized';
        }break;
        default:
        this.errorMessage=e.message;
          break;
      }
    }
    else
    {
      this.errorMessage=e.message
    }
        console.log(e);
    },

    rebindSequence(num){
      this.sequence.push(num);
    },

    deleteControl(e){
      alert(e);
      console.log(e.target.closest(e.target));
    }
  },
}
</script>

<style>

.deleteIconcss{
  cursor: pointer;
  display: inline-block;
}

/* Customize the label (the container) */
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

.info, .success, .warning, .error, .validation {
  width:10%;
  border: 1px solid;
  margin: 10px 0px;
  border-radius: 5%;
  padding: 15px 10px 15px 50px;
  background-repeat: no-repeat;
  background-position: 10px center;
}

.error{
  color: #D8000C;
  background-color: #FFBABA;
  background-image: url('https://i.imgur.com/GnyDvKN.png');
}

.grabbable {
    cursor: move; /* fallback if grab cursor is unsupported */
    cursor: grab;
    cursor: -moz-grab;
    cursor: -webkit-grab;
}

 /* (Optional) Apply a "closed-hand" cursor during drag operation. */
.grabbable:active {
    cursor: grabbing;
    cursor: -moz-grabbing;
    cursor: -webkit-grabbing;
}
    
#employees {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

.button {
  background-color: #04AA6D;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 5%;
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

.darkButton {background-color: #555555;color: #f2f2f2; border-radius: 4px;}

[contentEditable=false]:empty:not(:focus):before{
        content:attr(data-text)
    }
</style>
