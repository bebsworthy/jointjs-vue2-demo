<template>
  <div class="hello">
    
    <div id="canvas"></div>
    <!-- <div id="myholder-small"></div> -->
  </div>
</template>

<script>
  const svgPanZoom = require('svg-pan-zoom')
  const $ = require('jquery')
  const _ = require('lodash')
  const joint = require('jointjs')
  const backbone = require('backbone')
  import 'jointjs/dist/joint.css'


  joint.dia.Element.define('apsel.Task', {
      size: { width: 100, height: 30 },
      attrs: {
          text: { text: 'Task'  },
          body: {
            refWidth: '100%',
            refHeight: '100%',
            strokeWidth: '1',
            stroke: '#000000',
            fill: '#FFFFFF'
          },
          label: {
            refX: '50%',
            refY: '50%',
            textVerticalAnchor: 'middle',
            textAnchor: 'middle',
            fontSize: 14,
            fill: '#333333'
          },
          dot : {
            cx:'0',
            cy:'0',
            r:'10',
            stroke:'black',
            strokeWidth:'1',
            fill:'red'
          }
      }
  }, {
      markup: [{
          tagName: 'rect',
          selector: 'body'
      }, {
          tagName: 'text',
          selector: 'label'
      }, {
        tagName: 'circle',
        selector: 'dot'
    }]
  });

  export default {
    name: 'HelloWorld',
    data() {
      return {}
    },
    mounted() {

      // Create the graph
      var graph = new joint.dia.Graph;

      // Create the paper to display the graph
      var paper = new joint.dia.Paper({
        el: $('#canvas'),
        width: 800,
        height: 500,
        model: graph,
        gridSize: 10,
        drawGrid: true,
        //defaultRouter: { name: 'metro' },
        snapLinks: true,
        linkPinning: false,
      });

      // // Utility for zooming and panning
      // var svgZoom = svgPanZoom('#canvas svg', {
      //   center: false,
      //   zoomEnabled: true,
      //   panEnabled: true,
      //   controlIconsEnabled: false,
      //   fit: false,
      //   minZoom: 0.5,
      //   maxZoom:2,
      //   zoomScaleSensitivity: 0.5
      // });
      // // Disable panning and zooming when the graph is clicked
      // (function(){
      //   paper.on('cell:pointerdown', function(){ svgZoom.disablePan(); });
      //   paper.on('cell:pointerup', function(){ svgZoom.enablePan(); });
      //   paper.on('cell:pointerclick', function(e){
      //     // message.addClass('visible');
      //     // message.html(e.el.textContent+' clicked');
      //     // setTimeout(function(){  
      //     //     message.removeClass('visible');
      //     // }, 1000);
      //   });
      // })();


      // Link tool
      var verticesTool = new joint.linkTools.Vertices();
      var segmentsTool = new joint.linkTools.Segments();
      var sourceArrowheadTool = new joint.linkTools.SourceArrowhead();
      var targetArrowheadTool = new joint.linkTools.TargetArrowhead();
      var sourceAnchorTool = new joint.linkTools.SourceAnchor();
      var targetAnchorTool = new joint.linkTools.TargetAnchor();
      var boundaryTool = new joint.linkTools.Boundary();
      var removeButton = new joint.linkTools.Remove();

      var toolsView = new joint.dia.ToolsView({
          tools: [
              verticesTool, segmentsTool,
              sourceArrowheadTool, targetArrowheadTool,
              sourceAnchorTool, targetAnchorTool,
              boundaryTool, removeButton
          ]
      });


      // Set some event for it
      paper.on('link:mouseenter', function(linkView) {
          linkView.showTools();
      });

      paper.on('link:mouseleave', function(linkView) {
          linkView.hideTools();
      });

      // var paperSmall = new joint.dia.Paper({
      //   el: $('#myholder-small'),
      //   width: 600,
      //   height: 100,
      //   model: graph,
      //   gridSize: 1
      // });
      // paperSmall.scale(.3);
      // paperSmall.$el.css('pointer-events', 'none');
      var rect = new joint.shapes.apsel.Task({
        position: { x: 100, y: 30 },
      });
      
      var rect2 = rect.clone();
      rect2.translate(300);
      rect.attr({
          text: { text: 'My Task' }
      });
      var link = new joint.shapes.standard.Link({
        source: { id: rect.id },
        target: { id: rect2.id },
        attrs: {
          line: {
            connection: true,
            fill: 'none',
            stroke: 'orange',
            strokeWidth: 2,
            sourceMarker: {
              'type': 'circle',
              'r': 4,
              'fill': 'white',
              'stroke': 'orange',
              'stroke-width': '2'
            }
            // targetMarker: {
            //     'type': 'circle',
            //     'r': 4,
            //     'fill': 'white',
            //     'stroke': 'orange',
            //     'stroke-width': '2'
            // }
        },
        }
      });
      link.labels([
      // A Simple label  
      {
          attrs: {
            text: { text: 'Begin' }
          },
          position: { distance: 0.15 }
      },
      // A label with custom markup
      {
        markup: [
        {
          tagName: 'circle',
          selector: 'body'
        }, {
          tagName: 'text',
          selector: 'label'
        }, {
          tagName: 'circle',
          selector: 'asteriskBody'
        }, {
          tagName: 'text',
          selector: 'asterisk'
        }
    ],
    attrs: {
      label: {
        text: '½',
        fill: '#000000',
        fontSize: 14,
        textAnchor: 'middle',
        yAlignment: 'middle',
        pointerEvents: 'none'
      },
      body: {
        ref: 'label',
        fill: '#ffffff',
        stroke: '#000000',
        strokeWidth: 1,
        refR: 1,
        refCx: 0,
        refCy: 0
      },
      asterisk: {
        ref: 'label',
        text: '＊',
        fill: '#ff0000',
        fontSize: 8,
        textAnchor: 'middle',
        yAlignment: 'middle',
        pointerEvents: 'none',
        refX: 16.5,
        refY: -2
      },
      asteriskBody: {
        ref: 'asterisk',
        fill: '#ffffff',
        stroke: '#000000',
        strokeWidth: 1,
        refR: 1,
        refCx: '50%',
        refCy: '50%',
        refX: 0,
        refY: 0
      }
    },
          position: { distance: 0.5 }
      },
      // A simple label with an offset
      {
          attrs: {
            text: { text: 'End' }
          },
          position: { 
            distance: 0.85, 
            offset: {
              x: -40,
              y: 80
          } }
      }]);







    //   link.attr({
    //     line: {
    //         connection: true,
    //         stroke: '#333333',
    //         strokeWidth: 2,
    //         strokeLinejoin: 'round',
    //         targetMarker: {
    //             'type': 'path',
    //             'd': 'M 10 -5 0 0 10 5 z'
    //         }
    //     },
    //     wrapper: {
    //         connection: true,
    //         strokeWidth: 10,
    //         strokeLinejoin: 'round'
    //     }
    // });
      // link.attr({
      //     '.connection': { stroke: 'blue' },
      //     '.marker-source': { fill: 'red', d: 'M 10 0 L 0 5 L 10 10 z' },
      //     '.marker-target': { fill: 'yellow', d: 'M 10 0 L 0 5 L 10 10 z' }
      // });
      // link.set('vertices', [{ x: 300, y: 60 }, { x: 400, y: 60 }, { x: 400, y: 20 }])
      // link.set('smooth', true)
      graph.addCells([rect, rect2, link]);
      
      graph.on('all', function(eventName, cell) {
         //console.log(arguments,"------------");
      });
      rect.on('change:position', function(element) {
        // console.log(element.id, ':', element.get('position'),);
      });

      var linkView = link.findView(paper);
      linkView.addTools(toolsView);
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #myholder {
    outline: 1px solid red
  }

</style>