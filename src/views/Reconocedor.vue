<template>
  <div class="my-1 mb-3 mx-auto text-center justify-content-center bg-light px-4 py-3" :class="{'rounded':!mobile}">
    <h3 class="text-center py-2 mb-3 font-weight-bold">Reconocedor de palabras en autómatas</h3>
    <div class="input-group mb-3">
      <input type="text" class="form-control" placeholder="Palabra a analizar" v-model="input.text">
      <input type="text" class="form-control" placeholder="Resultado de Turing" v-if="input.turing" v-model="input.resultTuring">
      <div class="input-group-append">
        <button class="btn btn-success" @click="analize()">Analizar</button>
      </div>
    </div>
    <input type="text" class="form-control" v-model="input.eRechazo" placeholder="Estado de rechazo">
    <div class="col text-light rounded text-center my-2 py-1" :class="{'bg-success':result=='Reconoce', 'bg-danger':result=='No reconoce'}">{{result}}</div>
    <table class="table-responsive text-center align-middle my-auto mx-auto justify-content-center mt-2">
      <tr>
        <th class="border-0"></th>
        <th class="border border-dark p-0" v-for="(t,index) in transitions" :key="index">
          <div class="input-group">
            <input type="text" autocomplete="false" class="form-control text-center rounded-0 border-0" :class="{'font-weight-bold':t.value.length>0}" style="min-width:50px" v-model="t.value" @input="t.value?t.value=t.value:''">
            <div class="bg-white input-group-append mx-auto col-12 col-md-2 px-0">
              <i class="fas fa-trash clickeable p-2 text-danger border-left w-100 my-auto" @click="deleteTransition(t)"></i>
            </div>
          </div>
        </th>
      </tr>
      <tr v-for="(s,index) in states" :key="index">
        <th class="border border-dark my-auto align-middle p-0 align-middle p-0" style="min-width:50px" :class="{'text-dark bg-white':!s.end, 'bg-dark text-light':s.end }">
         <div class="row mx-auto my-auto align-middle p-1">
            <i class="fas fa-trash clickeable p-2 text-danger rounded mx-auto col-12 col-md order-2 border my-1 my-md-0" :class="{'col-md-12':transitions.length>5}" @click="deleteState(s)"></i>
            <p class="py-0 px-2 my-auto col-12 col-md order-1 order-md-2">{{s.value}}</p>
            <i class="fas fa-flag-checkered clickeable p-2 rounded mx-auto col-12 col-md order-3 border my-1 my-md-0" :class="{'bg-success text-light':s.end, 'col-md-12':transitions.length>5}" @click="s.end=!s.end"></i>
         </div>
        </th>
        <th class="border border-dark p-0" v-for="(t,index) in table[states.indexOf(s)]" :key="index">
          <input type="text" autocomplete="false" placeholder="Próximo estado" class="form-control border-0 text-center rounded-0" :class="{'font-weight-bold':t.nextState && t.nextState.length>0}" v-model="t.nextState" @input="t.nextState = t.nextState.toUpperCase()">
          <input type="text" autocomplete="false" placeholder="Leer de pila" class="form-control text-center rounded-0" :class="{'border-top border-bottom border-left-0 border-right-0': input.pila, 'font-weight-bold':t.read && t.read.length>0}" v-model="t.read" v-if="input.pila">
          <input type="text" autocomplete="false" placeholder="Escribir en pila" class="form-control border-0 text-center rounded-0" :class="{'font-weight-bold':t.write && t.write.length>0}" v-model="t.write" v-if="input.pila">
          <input type="text" autocomplete="false" placeholder="Reemplazar en la palabra" class="form-control border-0 text-center rounded-0" :class="{'border-top border-left-0 border-right-0': input.turing, 'font-weight-bold':t.write && t.write.length>0}" v-model="t.write" v-if="input.turing">
          <input type="number" autocomplete="false" placeholder="Incremento + o - algo" class="form-control border-0 text-center rounded-0" :class="{'border-top border-left-0 border-right-0': input.turing, 'font-weight-bold':t.move}" v-model="t.move" v-if="input.turing">
        </th>
      </tr>
    </table>
    <div class="row col-12 mx-auto text-center my-3 text-center">
      <button class="btn col-lg col-12 btn-success my-2 mx-auto m-md-2" @click="addState">+1 Estado</button>
      <button class="btn col-lg col-12 btn-success my-2 mx-auto m-md-2" @click="addTransition">+1 Transición</button>
    </div>
    <div class="col-12 my-2">
      <div class="custom-control custom-switch">
        <input type="checkbox" class="custom-control-input" id="pila" v-model="input.pila" @click="input.turing=false">
        <label class="custom-control-label" for="pila">Agregar pila</label>
      </div>  
    </div>
    <div class="col-12 my-2">
      <div class="custom-control custom-switch">
        <input type="checkbox" class="custom-control-input" id="turing" v-model="input.turing" @click="input.pila=false">
        <label class="custom-control-label" for="turing">Máquina de Turing</label>
      </div>  
    </div>
    <div class="container my-2 text-center mx-auto">
      <button class="btn btn-warning rounded-pill d-block mx-auto my-2 font-weight-bold px-4 py-2" @click="showAutomata = !showAutomata">
        Mostrar autómata
      </button>
      <button class="btn btn-secondary rounded-pill d-block mx-auto my-2 font-weight-bold px-4 py-2" @click="showHistory = true">
          Mostrar historial
      </button>
      <button class="btn btn-info rounded-pill d-block mx-auto my-2 font-weight-bold px-4 py-2" @click="showInstructions = true">
        Mostrar instrucciones
      </button>
    </div>
    <template v-if="table">
      {{c()}}
    </template>
    <b-modal centered title="Instrucciones" hide-footer v-model="showInstructions">
      <p class="my-1 text-center">Las transiciones son editables, para que puedas realizar el automata que desees</p>
      <p class="my-1 text-center"><i class="fas fa-flag-checkered border p-2 rounded"></i> Estado no terminal <i class="fas fa-flag-checkered border p-2 rounded"></i></p>
      <p class="my-1 text-center"><i class="fas fa-flag-checkered border p-2 rounded bg-success text-light"></i> Estado terminal <i class="fas fa-flag-checkered border p-2 rounded bg-success text-light"></i></p>
      <p class="my-1 text-center"><i class="fas fa-trash border p-2 rounded text-danger"></i> Eliminar estado/transición <i class="fas fa-trash border p-2 rounded text-danger bg-dark"></i></p>
    </b-modal>
    <b-modal id="showAutomata" size="lg" centered title="Autómata" class="mx-auto" hide-footer v-model="showAutomata">
      <template  v-if="states.length>0" >
        <Automata :table="table" :states="states" :key="showAutomata" />
        <div class="text-center row mx-auto">
          <p class="my-1 mx-auto"><i class="fas fa-circle"></i> Estado terminal</p>
          <p class="my-1 mx-auto"><i class="far fa-circle"></i> Estado no terminal</p>
        </div>
      </template>
      <p class="my-3 text-center" v-else>Ingrese una tabla de transiciones</p>
    </b-modal>
    <b-modal centered size="lg" title="Historial" hide-footer v-model="showHistory">
      <div class="input-group mb-3">
        <input type="text" class="form-control" v-model="input.tableName">
        <div class="input-group-append">
          <button class="btn btn-success" @click="addHistory()">Añadir</button>
        </div>
      </div>
      <div class="py-2 mx-auto text-center" :class="{'border-bottom':index!=history.length-1}" v-for="(table,index) in history" :key="index">
        <p class="my-0 d-inline-block align-middle mx-2">{{table.name}}</p>
        <button class="btn btn-info d-inline-block mx-2" @click="tableHistoryShow = table" v-b-modal.showTable>Ver tabla y autómata</button>
        <button class="btn btn-success" @click="setActual(table); showHistory=false">Cargar</button>
      </div>
    </b-modal>
    <b-modal id="showTable" centered :title="tableHistoryShow.name" v-if="tableHistoryShow">
      <table class="table-responsive text-center align-middle justify-content-center m-2 p-2" v-if="tableHistoryShow">
        <tr>
          <th></th>
          <th class="border border-dark p-0" v-for="(t,index) in tableHistoryShow.transitions" :key="index">
            <div class="input-group">
              <p type="text" class="form-control text-center rounded-0 border-0" :class="{'font-weight-bold':t.value.length>0}" style="min-width:50px">{{t.value}}</p>
            </div>
          </th>
        </tr>
        <tr v-for="(s,index) in tableHistoryShow.states" :key="index">
          <th class="border border-dark my-auto align-middle p-0 align-middle p-0" style="min-width:50px" :class="{'text-dark':!s.end, 'bg-dark text-light':s.end }">
          <div class="row mx-auto my-auto align-middle p-1">
            <p class="py-0 px-2 my-auto col-12 col-md order-1 order-md-2">{{s.value}}</p>
          </div>
          </th>
          <th class="border border-dark p-0" v-for="(t,index) in tableHistoryShow.table[tableHistoryShow.states.indexOf(s)]" :key="index">
            <p type="text" placeholder="Próximo estado" class="form-control border-0 text-center rounded-0 my-0" :class="{'font-weight-bold':t.nextState && t.nextState.length>0}">{{t.nextState}}</p>
            <p type="text" placeholder="Leer de pila" class="form-control text-center rounded-0 my-0" :class="{'border-top border-bottom border-left-0 border-right-0': tableHistoryShow.pila, 'font-weight-bold':t.read && t.read.length>0}" v-if="tableHistoryShow.pila">{{t.read}}</p>
            <p type="text" :placeholder="{'Escribir en pila':tableHistoryShow.pila, 'Escribir en palabra':tableHistoryShow.turing}" class="form-control border-0 text-center rounded-0 my-0" :class="{'border-top border-left-0 border-right-0': tableHistoryShow.turing, 'font-weight-bold':t.write && t.write.length>0}" v-if="tableHistoryShow.pila || tableHistoryShow.turing">{{t.write}}</p>
          </th>
        </tr>
      </table>
      <Automata :table="tableHistoryShow.table" :states="tableHistoryShow.states" />
      <template v-slot:modal-footer>
        <b-button size="sm" variant="success" @click="setActual(tableHistoryShow);$bvModal.hide('showTable'); showHistory=false;">
          Cargar tabla
        </b-button>
      </template>
    </b-modal>
  </div>
</template>

<script>
import Automata from '@/components/Automata.vue'
export default {
  name: 'Reconocedor',
  components:{Automata},
  props: [],
  data(){
    var turing= {
      table: [ [ { "nextState": "S1", "read": null, "write": "X", "move": 1, "state": "S0", "transition": "a" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S0", "transition": "b" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S0", "transition": "c" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S0", "transition": "X" }, { "nextState": "S6", "read": null, "write": "Y", "move": 1, "state": "S0", "transition": "Y" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S0", "transition": "Z" } ], [ { "nextState": "S1", "read": null, "write": "a", "move": 1, "state": "S1", "transition": "a" }, { "nextState": "S2", "read": null, "write": "Y", "move": 1, "state": "S1", "transition": "b" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S1", "transition": "c" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S1", "transition": "X" }, { "nextState": "S4", "read": null, "write": "Y", "move": 1, "state": "S1", "transition": "Y" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S1", "transition": "Z" } ], [ { "nextState": null, "read": null, "write": null, "move": null, "state": "S2", "transition": "a" }, { "nextState": "S2", "read": null, "write": "b", "move": 1, "state": "S2", "transition": "b" }, { "nextState": "S3", "read": null, "write": "Z", "move": -1, "state": "S2", "transition": "c" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S2", "transition": "X" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S2", "transition": "Y" }, { "nextState": "S5", "read": null, "write": "Z", "move": 1, "state": "S2", "transition": "Z" } ], [ { "nextState": "S3", "read": null, "write": "a", "move": -1, "state": "S3", "transition": "a" }, { "nextState": "S3", "read": null, "write": "b", "move": -1, "state": "S3", "transition": "b" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S3", "transition": "c" }, { "nextState": "S0", "read": null, "write": "X", "move": 1, "state": "S3", "transition": "X" }, { "nextState": "S3", "read": null, "write": "Y", "move": -1, "state": "S3", "transition": "Y" }, { "nextState": "S3", "read": null, "write": "Z", "move": -1, "state": "S3", "transition": "Z" } ], [ { "nextState": null, "read": null, "write": null, "move": null, "state": "S4", "transition": "a" }, { "nextState": "S2", "read": null, "write": "Y", "move": 1, "state": "S4", "transition": "b" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S4", "transition": "c" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S4", "transition": "X" }, { "nextState": "S4", "read": null, "write": "Y", "move": 1, "state": "S4", "transition": "Y" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S4", "transition": "Z" } ], [ { "nextState": null, "read": null, "write": null, "move": null, "state": "S5", "transition": "a" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S5", "transition": "b" }, { "nextState": "S3", "read": null, "write": "Z", "move": -1, "state": "S5", "transition": "c" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S5", "transition": "X" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S5", "transition": "Y" }, { "nextState": "S5", "read": null, "write": "Z", "move": 1, "state": "S5", "transition": "Z" } ], [ { "nextState": null, "read": null, "write": null, "move": null, "state": "S6", "transition": "a" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S6", "transition": "b" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S6", "transition": "c" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S6", "transition": "X" }, { "nextState": "S6", "read": null, "write": "Y", "move": 1, "state": "S6", "transition": "Y" }, { "nextState": "S7", "read": null, "write": "Z", "move": 1, "state": "S6", "transition": "Z" } ], [ { "nextState": null, "read": null, "write": null, "move": null, "state": "S7", "transition": "a" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S7", "transition": "b" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S7", "transition": "c" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S7", "transition": "X" }, { "nextState": null, "read": null, "write": null, "move": null, "state": "S7", "transition": "Y" }, { "nextState": "S7", "read": null, "write": "Z", "move": 1, "state": "S7", "transition": "Z" } ] ],
      states: [{value:"S0",end:false} ,{value:"S1",end:false} ,{value:"S2",end:false} ,{value:"S3",end:false} ,{value:"S4",end:false} ,{value:"S5",end:false} ,{value:"S6",end:false} ,{value:"S7",end:true}],
      transitions:  [{value:"a"} ,{value:"b"} ,{value:"c"} ,{value:"X"} ,{value:"Y"} ,{value:"Z"}],
      turing:true,
      pila:false,
      name: "Turing que recibe abc y devuelve XYZ",
      text: "aaabbbccc",
      resultTuring: "XXXYYYZZZ"
    }
    return {
      input:{
        text: '',
        tableName: '',
        pila:false,
        turing: false,
        resultTuring: '',
        eRechazo: ''
      },
      result:'',  
      table:[],
      states:[],
      transitions:[],
      timer: null,
      delay: 700,
      clicks: 0,
      history:[turing],
      showHistory:false,
      showInstructions:false,
      showAutomata:false,
      tableHistoryShow: null,
      mobile: window.innerWidth<576,
    }
  },
  methods:{
    c(){
      var tabla = this.table;
      var temp = '{';
      for(var i = 0; i<tabla.length; i++){
        var tempRow = '';
        let row = tabla[i];
        for(var j = 0; j<row.length; j++){
          let col = row[j];
          tempRow += `{${(col.nextState?col.nextState:this.input.eRechazo).replace("S","")}}${j==(row.length-1)?"":","}`;
        }
        console.log(i==(tabla.length-1));
        temp+= `{${tempRow}}${i==(tabla.length-1) ? "": "," }`;
      }
      temp += "}";
      return temp;
    },
    adapt(val){
      var states = val.states;
      var transitions = val.transitions;
      var temp = val.table;
      for(var i = 0; i<temp.length; i++){
        for(var j = 0; j< temp[i].length; j++){
          temp[i][j]["state"] = states[i].value;
          temp[i][j]["transition"] = transitions[j].value;
        }
      }
      return temp;
    },
    setActual(el){
      this.table = this.copyTable(el.table);
      this.states = this.copyTransitionStates(el.states);
      this.transitions = this.copyTransitionStates(el.transitions);
      this.input.pila = el.pila?true:false;
      this.input.turing = el.turing?true:false;
      this.input.text = el.text?el.text:"";
      this.input.resultTuring = el.resultTuring?el.resultTuring:"";
    },
    copyTable(val){
      var temp=[];
      val.forEach(el=>{
        let tempEl = [];
        el.forEach(el2 => {
          let tempEl2 ={
            nextState: el2.nextState,
            read: el2.read,
            write: el2.write,
            move: el2.move,
            state: el2.state,
            transition: el2.transition
          }
          tempEl.push(tempEl2);
        });
        temp.push(tempEl)
      })
      return temp;
    },
    copyTransitionStates(val){
      var temp = [];
      val.forEach(el=>{
        let tempEl = {
          value:el.value,
          end: el.end?el.end:false
        }
        temp.push(tempEl)
      });
      return temp;
    },
    addHistory(){
      if(this.table.length>0){
        let temp = {
          name: this.input.tableName,
          table: this.copyTable(this.table),
          transitions: this.copyTransitionStates(this.transitions),
          states: this.copyTransitionStates(this.states),
          pila: this.input.pila?true:false,
          turing: this.input.turing?true:false,
        }
        this.history = [temp].concat(this.history);
        this.input.tableName = '';
      }
    },
    deleteTransition(transition){
      if(confirm("Deseas eliminar la transición " + transition.value + "?")){
        if(this.transitions.length == 1){
          this.table = [];
          this.states = [];
          this.transitions = [];
        }else{
          let index = this.findIndex(this.transitions, transition.value);
          this.transitions.splice(index ,1);
          this.table.forEach(row=>{
            row.splice(index, 1);
          })
        } 
      }
    },
    deleteState(state){
      if(confirm("Deseas eliminar el estado " + state.value + "?")){
        if(this.states.length == 1){
          this.table = [];
          this.states = [];
          this.transitions = [];
        }else{
          let index = this.findIndex(this.states, state.value);
          this.states.splice(index ,1);
          this.table.splice(index, 1);
        } 
      }
    },
    addTransition(){
      var nextValue = this.nextLetter(this.transitions[this.transitions.length-1]);
      this.transitions.push({
        value: nextValue
      })
      var cont = 0;
      this.table.forEach(e=>{
        e.push({
          nextState: null,
          read: null,
          write: null,
          move: null,
          transition: nextValue,
          state: this.states[cont].value,
        })
        cont++;
      })
      if(this.states.length==0){
        this.addState()
      }
    },
    addState(){
      var nextState = "S" + (this.states.length>0?this.states.length:0);
      this.states.push({
        value: nextState,
        end: false
      });
      var temp = [];
      for(var i = 0; i<this.transitions.length; i++){
        let nextValue = this.transitions[i].value;
        temp.push({
          nextState:null,
          read: null,
          write: null,
          move: null,
          transition: nextValue,
          state: nextState
        })
      }
      this.table.push(temp)
      if(this.transitions.length==0){
        this.addTransition()
      }
      this.input.state = ''
    },
    nextLetter(c){
      return c && c.value?String.fromCharCode(c.value.charCodeAt(0) + 1):"a";
    },
    findIndex(lista, buscar){
      for(var i = 0; i<lista.length; i++){
        if(lista[i].value == buscar){
          return i
        }
      }
      return (-1)
    },
    analizePila(){
      var pila = [];
      var palabra = this.input.text;
      var tabla = this.table;
      var estado = this.states[0].value;
      var col, lin;
      var reconoce = true;
      for(var i = 0; i<palabra.length; i++){
        let letra = palabra[i];
        if(reconoce){
          col = this.findIndex(this.transitions, letra);
          lin = this.findIndex(this.states, estado);
          let celda = col > -1 && lin > -1?tabla[lin][col]:null;
          estado = celda?celda.nextState:null;
          if(celda.read && pila.pop()!=celda.read){
            reconoce = false;
          }
          if(celda.write){
            pila.push(celda.write)
          }
          if(!estado){
            reconoce = false;
          }
        }
      }
      let estadoFinal = this.states[this.findIndex(this.states, estado)];
      return reconoce && estadoFinal && estadoFinal.end && pila.length==0?"Reconoce":"No reconoce"
    },
    analizeTuring(){
      var col, lin, letra;
      var palabra = this.input.text.split("");
      var tabla = this.table;
      var estado = this.states[0].value;
      var reconoce = true;
      var i = 0;
      var max = this.input.text.length;
      do{
        letra = palabra[i];
        col = this.findIndex(this.transitions, letra);
        lin = this.findIndex(this.states, estado);
        let temp = tabla[lin][col];
        estado = temp.nextState;
        let incremento = temp.move;
        if(temp.write){
          palabra[i] = temp.write;
        }else{
          reconoce = false;
        }
        i+=parseInt(incremento, 10);
      }while(i<max && estado && reconoce)
      let estadoFinal = this.states[this.findIndex(this.states, estado)];
      // console.log("Reconoce: ", reconoce);
      // console.log("Palabra: ", palabra.join(""), palabra.join("") == this.input.resultTuring);
      // console.log("Estado final: ", estadoFinal)
      return reconoce && palabra.join("") == this.input.resultTuring && estadoFinal && estadoFinal.end?"Reconoce":"No reconoce"
    },
    analizeOther(){
      var palabra = this.input.text;
      var tabla = this.table;
      var estado = this.states[0].value;
      var col, lin;
      var reconoce = true;
      for(var i = 0; i<palabra.length; i++){
        let letra = palabra[i];
        if(reconoce){
          col = this.findIndex(this.transitions, letra);
          lin = this.findIndex(this.states, estado);
          let celda = col > -1 && lin > -1?tabla[lin][col]:null;
          estado = celda?celda.nextState:null;
          if(!estado){
            reconoce = false;
          }
        }
      }
      let estadoFinal = this.states[this.findIndex(this.states, estado)];
      return reconoce && estadoFinal && estadoFinal.end ?"Reconoce":"No reconoce"
    },
    analize(){
      this.result="";
      if(this.table.length>0){
        if(this.input.text.length==0){
          this.result = this.transitions.find(el=>el.value=="") && this.states[0].end?"Reconoce":"No reconoce";
        }else if(this.input.pila){
          this.result = this.analizePila();
        }else if(this.input.turing){
          this.result = this.analizeTuring();
        }else{
          this.result = this.analizeOther();
        }
      }
    }
  }
}
</script>

<style scoped>
.border-left-top{
  border-top-left-radius: 0.25rem;
}
.border-right-top{
  border-top-left-radius: 0.25rem;
}
.clickeable{
  cursor: pointer;
}
.border-left-0{
  border-left-width: 0px;
}
.border-right-0{
  border-right-width: 0px;
}
</style>
