<template>
    <div class="editor-container">
        <div class="editor-left">
            <v-navigation-drawer
              theme="dark"
              app
              mini-variant
              rail
              permanent
              elevation="2"
            >
                <v-list-item
                  nav
                  prepend-avatar="https://xsgames.co/randomusers/avatar.php?g=pixel"
                ></v-list-item>

                <v-divider></v-divider>

                <v-list density="compact" nav>
                    <v-list-item
                      prepend-icon="mdi mdi-format-text"
                      @click="selectPanel('text')" :value="'text'" :class="{ 'active': activePanel === 'text' }"
                    ></v-list-item>

                    <v-list-item prepend-icon="mdi mdi-shape"
                                 @click="selectPanel('shape')" :value="'shape'" :class="{ 'active': activePanel === 'shape' }"
                    ></v-list-item>
                    <v-list-item prepend-icon="mdi mdi-draw"
                                 @click="selectPanel('draw')" :value="'draw'" :class="{ 'active': activePanel === 'draw' }"
                    ></v-list-item>
                    <v-list-item prepend-icon="mdi mdi-download"
                                 @click="selectPanel('download')" :value="'download'" :class="{ 'active': activePanel === 'download' }"
                    ></v-list-item>
                </v-list>
            </v-navigation-drawer>
            <TextPanel v-if="activePanel === 'text'" @add-text="addText" @apply-text-tool="applyTextTool" />
            <ShapePanel v-if="activePanel === 'shape'" @add-shape="addShape" @change-shape-color="changeShapeColor" />
            <DrawPanel v-if="activePanel === 'draw'" @start-drawing="startDrawing" @stop-drawing="stopDrawing" @clear-canvas="clearCanvas"
                       @change-brush-color="changeBrushColor" @change-brush-width="changeBrushWidth" @change-brush-type="changeBrushType" />

            <DownloadPanel v-if="activePanel === 'download'" @load="Load" @download="Download" />

        </div>


        <div class="editor-center">
            <div class="editor-canvas-area">
                <!-- Barre d'outils du canevas -->
                <div class="editor-canvas-toolbar">
                    <!-- Contenu de la barre d'outils pour modifier le texte -->

                </div>
                <!-- Canevas pour afficher le poster -->
                <v-row align="center" justify="center">
                    <v-col cols="12" sm="10" md="8" lg="6">
                        <canvas ref="canvas" width="550" height="700" class="editor-canvas"></canvas>
                    </v-col>
                </v-row>
            </div>

            <!-- Footer pour les boutons d'outils de zoom et d'ajout de poster -->
            <v-footer app>
                <v-row align="center">
                    <v-col>
                        <v-btn icon color="primary">
                            <v-icon>mdi mdi-plus</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-footer>
        </div>
    </div>
</template>

<script>
import { fabric } from "fabric";
import TextPanel from "../components/Panels/TextPanel.vue";
import ShapePanel from "../components/Panels/ShapePanel.vue";
import DrawPanel from "../components/Panels/DrawPanel.vue";
import DownloadPanel from "../components/Panels/DownloadPanel.vue";
import data from "../Data/datajson.json";
import TestPanel from "../components/Panels/TestPanel.vue";

let canvas = null;
  export default {
      data() {
          return {
              canvasWidth: 550, // Largeur du canevas
              canvasHeight: 700, // Hauteur du canevas
              activePanel: 'text',
              picker: null,
              brushType: "pencil",
              brushColor: "#000000",
              brushWidth: 5,
              isDrawingMode: false,
              isDrawing: false,
              lastPointer: null,
          };
      },
      mounted() {
         canvas = new fabric.Canvas(this.$refs.canvas, {
             backgroundColor: '#ffffff',
             width: this.canvasWidth,
             height: this.canvasHeight,
             hoverCursor: 'pointer',
             selection: true,
             selectionBorderColor: 'blue',
             preserveObjectStacking: true,
         });

          canvas.on("selection:created", function() {
              const activeObject = canvas.getActiveObject();
              if (activeObject && activeObject.type === "i-text") {
                  activeObject.set({
                      cornerStyle: "circle",
                      cornerColor: "rgb(62, 155, 246)",
                      cornerSize: 12,
                  });
                  canvas.renderAll();
              }
          });

          canvas.on('selection:cleared', function(e) {
              if (e.target.type === 'i-text') {
                  e.target.set({
                      strokeDashArray: null, // Réinitialiser la bordure pointillée
                      cornerStyle: 'rect', // Réinitialiser le style de coin
                      cornerSize: 6 // Réinitialiser la taille des coins
                  });
                  canvas.renderAll();
              }
          });


          canvas.on('selection:created', (e) => {
              console.log('selection:created', e)
          });

          canvas.on('mouse:down', function(options) {
              if (options.target) {
                  console.log('an object was clicked! ', options.target.type);
              }
          });


          document.addEventListener("keydown", (e) => {
              if (e.code === "Delete") {
                  const activeObject = canvas.getActiveObject();
                  if (activeObject) {
                      canvas.remove(activeObject);
                      canvas.renderAll();
                  }
              }
          });

          this.initBrush();

      },
      components: { TestPanel, DownloadPanel, DrawPanel, ShapePanel, TextPanel },
      methods: {
          selectPanel(panel) {
              this.activePanel = panel;
          },
          addText(e, value) {
              let text = null;
              if (value === "title") {
                  text = this.addTitle();
              } else if (value === "subtitle") {
                  text = this.addSubtitle();
              } else if (value === "paragraph") {
                  text = this.addParagraph();
              }
              canvas.add(text);
              canvas.setActiveObject(text);
          },
          addTitle() {
              return new fabric.IText("Titre", {
                  left: 230,
                  top: 100,
                  fontFamily: "Roboto",
                  fontSize: 36,
                  fill: "#000000",
                  fontWeight: "bold",
                  underline: false,
                  linethrough: false,
                  fontStyle: "normal",
                  textAlign: "left",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },

          addSubtitle() {
              return new fabric.IText("Sous-titre", {
                  left: 230,
                  top: 200,
                  fontFamily: "Roboto",
                  fontSize: 24,
                  fill: "#000000",
                  fontWeight: "bold",
                  underline: false,
                  linethrough: false,
                  fontStyle: "normal",
                  textAlign: "left",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },

          addParagraph() {
              return new fabric.IText("Paragraphe", {
                  left: 230,
                  top: 300,
                  fontFamily: "Roboto",
                  fontSize: 18,
                  fill: "#000000",
                  fontWeight: "normal",
                  underline: false,
                  linethrough: false,
                  fontStyle: "normal",
                  textAlign: "left",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },
          applyTextTool(e, tool) {
              const activeObject = canvas.getActiveObject();
              console.log('activeObject', activeObject)
              console.log('type', activeObject.type)
              if (activeObject && activeObject.type === "i-text") {
                  switch (tool.value) {
                      case "color":
                          activeObject.set("fill", "#FF0000");
                          break;
                      case "size":
                          activeObject.set("fontSize", 36);
                          break;
                      case "bold":
                          activeObject.set("fontWeight", "bold");
                          break;
                      case "italic":
                          activeObject.set("fontStyle", "italic");
                          break;
                      case "underline":
                          activeObject.set("underline", "true");
                          break;
                      case "linethrough":
                          activeObject.set("linethrough", "true");
                          break;
                      default:
                          break;
                  }
                  canvas.renderAll();
              }
          },

          addShape(e, value) {
              let shape = null;
              if (value === "square") {
                  shape = this.addSquare();
              } else if (value === "rectangle") {
                  shape = this.addRectangle();
              } else if (value === "circle") {
                  shape = this.addCircle();
              } else if (value === "triangle") {
                  shape = this.addTriangle();
              } else if (value === "ellipse") {
                  shape = this.addEllipse();
              }
              canvas.add(shape);
              canvas.setActiveObject(shape);
          },
          addSquare() {
              return new fabric.Rect({
                  left: 230,
                  top: 100,
                  width: 100,
                  height: 100,
                  fill: "#000000",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },
          addRectangle() {
              return new fabric.Rect({
                  left: 230,
                  top: 100,
                  width: 200,
                  height: 100,
                  fill: "#000000",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },
          addCircle() {
              return new fabric.Circle({
                  left: 230,
                  top: 100,
                  radius: 50,
                  fill: "#000000",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },
          addTriangle() {
              return new fabric.Triangle({
                  left: 230,
                  top: 100,
                  width: 100,
                  height: 100,
                  fill: "#000000",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },
          addEllipse() {
              return new fabric.Ellipse({
                  left: 230,
                  top: 100,
                  rx: 50,
                  ry: 100,
                  fill: "#000000",
                  hasRotatingPoint: false,
                  cornerStyle: "circle",
                  cornerColor: "rgb(62, 155, 246)",
                  cornerSize: 12,
              });
          },
          startDrawing() {
              canvas.isDrawingMode = true;
              canvas.freeDrawingBrush.color = this.brushColor;
              canvas.freeDrawingBrush.width = this.brushWidth;
              if (this.brushType === "pencil") {
                  canvas.freeDrawingBrush = new fabric.PencilBrush(canvas);
              } else if (this.brushType === "brush") {
                  canvas.freeDrawingBrush = new fabric.CircleBrush(canvas);
              } else if (this.brushType === "circle") {
                  canvas.freeDrawingBrush = new fabric.SprayBrush(canvas);
              } else if (this.brushType === "spray") {
                  canvas.freeDrawingBrush = new fabric.PatternBrush(canvas);
              } else if (this.brushType === "pattern") {
                  canvas.freeDrawingBrush = new fabric.PatternBrush(canvas);
              }

              canvas.renderAll();
          },

          stopDrawing() {
              canvas.isDrawingMode = false;
          },

          changeShapeColor(color) {
              const activeObject = canvas.getActiveObject();
              if (activeObject) {
                  activeObject.set("fill", color);
                  canvas.renderAll();
              }
          },
          changeBrushColor(color) {
              this.brushColor = color;
              canvas.freeDrawingBrush.color = color;
          },

          changeBrushWidth(width) {
              this.brushWidth = width;
              canvas.freeDrawingBrush.width = width;
          },
          changeBrushType(brushType) {
              this.brushType = brushType;
              const capitalizedBrushType = brushType.charAt(0).toUpperCase() + brushType.slice(1);
              canvas.freeDrawingBrush = new fabric[capitalizedBrushType + "Brush"](canvas);
          },

          clearCanvas() {
              canvas.clear();
              canvas.renderAll();
          },

          initBrush() {
              canvas.freeDrawingBrush.color = this.brushColor;
              canvas.freeDrawingBrush.width = this.brushWidth;
          },

          Load(e, value) {
              console.log("value", value);
              if (value === "json") {
                  this.loadJson();
              } else if (value === "svg") {
                  this.loadSvg();
              }
          },
          loadJson() {
              const json = JSON.stringify(data);
              canvas.loadFromJSON(json, function () {
                  canvas.renderAll();
              });
          },
          loadSvg() {
              const svgData = `<?xml version="1.0" standalone="no"?><!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd"><svg width="100%" height="100%" version="1.1" xmlns="http://www.w3.org/2000/svg"><rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0)"/></svg>`
              fabric.loadSVGFromString(svgData, function (objects, options) {
                  const obj = fabric.util.groupSVGElements(objects, options);
                  canvas.add(obj).renderAll();
              });
          },

          Download(e, value) {
              console.log("value", value);
              if (value === "json") {
                  this.downloadJson();
              } else if (value === "svg") {
                  this.downloadSvg();
              } else if (value == "image") {
                  this.downloadImage();
              }
          },
          //exporter en jon
          downloadJson() {
              console.log("downloadJson")
              const json = JSON.stringify(canvas);
              const blob = new Blob([json], { type: "application/json" });
              const url = URL.createObjectURL(blob);
              const a = document.createElement("a");
              document.body.appendChild(a);
              a.download = "canvas.json";
              a.href = url;
              a.click();
              document.body.removeChild(a);
          },

          downloadSvg() {
              console.log("downloadSvg")
              const svg = canvas.toSVG();
              const blob = new Blob([svg], { type: "image/svg+xml" });
              const url = URL.createObjectURL(blob);
              const a = document.createElement("a");
              document.body.appendChild(a);
              a.download = "canvas.svg";
              a.href = url;
              a.click();
              document.body.removeChild(a);
          },
          downloadImage() {
              console.log("downloadImage")
              const image = canvas.toDataURL({
                  format: "png",
                  multiplier: 4,
                  quality: 1,
              });
              const blob = new Blob([image], { type: "image/png" });
              const url = URL.createObjectURL(blob);
              const a = document.createElement("a");
              document.body.appendChild(a);
              a.download = "canvas.png";
              a.href = url;
              a.click();
              document.body.removeChild(a);
          }

      },
  };

</script>

<style scoped>
.editor-container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    height: 100%;
}

.editor-left {
    flex: 0 0 auto;
}

.editor-canvas {
    margin: 10px;
    border: 1px solid #ccc;
}

.editor-center {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1 1 auto;
    height: 100%;
}

.editor-canvas-area {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1 1 auto;
    height: 100%;
}

canvas.editor-canvas {
    border: 2px solid #333;
    cursor: pointer;
    background-color: #f1f1f1;
    margin: 20px;
    box-shadow: 0 0 10px #888888;
}
</style>
