<template>
    <div>
        <div id="automata" class="container" style="height:50vh"></div>
    </div>
</template>

<script>
// import { Machine } from 'xstate';
import go from 'gogogojsvue';

export default {
    props:['table', 'states'],
    data(){
        return{
            automata:null,
            datos: null
        }
    },
    mounted(){
        var $ = go.GraphObject.make;
        this.automata = $(go.Diagram, "automata",{
                  layout: $(go.LayeredDigraphLayout)
                });
        this.automata.nodeTemplate =
            $(go.Node, "Auto",
            { fromSpot: go.Spot.RightSide,
            toSpot: go.Spot.NotRightSide },
            $(go.Shape, "Ellipse", new go.Binding("fill", "color")),
            $(go.TextBlock, { margin: 5 },
                new go.Binding("text", "key"),
                new go.Binding("stroke", "textColor"))
            );

        this.automata.linkTemplate =$(go.Link,
            { routing: go.Link.Orthogonal,  // Orthogonal routing
            corner: 10,
            curve: go.Link.JumpOver},                 // with rounded corners
            $(go.Shape),
            $(go.Shape, { toArrow: "Standard" }),
            $(go.Panel, "Auto",  // this whole Panel is a link label
                $(go.Shape, "Ellipse", { fill: "gray", stroke: "gray" }),
                $(go.TextBlock, { margin: 0 },
                new go.Binding("text", "text"))
            )
        )

        var datos = this.getTable(this.table);
        this.automata.model = new go.GraphLinksModel(
            datos.states,
            datos.transitions
        );

    },
    watch:{
        table: function (newVal){
            var datos = this.getTable(newVal);
            console.log(datos);
            this.automata.model = new go.GraphLinksModel(
                datos.states,
                datos.transitions
            );
        }
    },
    methods:{
        getTable(val){
            console.log(val)
            var states = [];
            var transitions = [];
            this.states.forEach(sta => {
                states.push({key: sta.value,
                color: sta.end?"black":"white",
                textColor: sta.end?"white":"black"
            });
            })
            val.forEach(sta => {
                sta.forEach(tran => {
                    if(tran.nextState){
                        console.log(tran)
                        transitions.push({
                            text: tran.transition,
                            from: tran.state,
                            to: tran.nextState
                        })
                    }
                })

            })
            return{
                states: states,
                transitions: transitions
            }
        }
    }
}
</script>