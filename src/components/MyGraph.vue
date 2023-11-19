<template>
  <div>
    <div id="cy"></div>
  </div>
</template>

<script>
import cytoscape from 'cytoscape';

function generateColor(name) {
  const number = name.match(/\d+/); // находим цифру в названии
  if (number) {
    const colorNumber = parseInt(number[0]); // преобразуем найденную цифру в число
    switch (colorNumber) {
      case 1:
        return 'red'; // красный цвет
      case 2:
        return 'blue'; // синий цвет
      case 3:
        return 'green'; // зеленый цвет
      case 4:
        return 'lightblue'; // голубой цвет
      case 5:
        return 'cyan'; // цвет бирюзовый
      case 6:
        return 'yellow'; // желтый цвет
      case 7:
        return 'gray'; // серый цвет
      case 8:
        return 'purple'; // фиолетовый цвет
      default:
        return 'black'; // возвращаем черный цвет по умолчанию
    }
  } else {
    return 'black'; // возвращаем черный цвет, если цифра не найдена
  }

}

export default {
  mounted() {
    this.fetchGraphData();
  },
  methods: {
    async fetchGraphData() {
      try {
        const response = await import('./data.json'); // Replace with the path to your JSON file
        const data = response.default;

        this.initGraph(data);
      } catch (error) {
        console.error('Error fetching or parsing JSON data:', error);
      }
    },
    initGraph(data) {
      cytoscape({
        container: document.getElementById('cy'),
        elements: data,
        layout: {          name: 'cose',
          idealEdgeLength: 500,
          nodeOverlap: 90000,
          refresh: 20,
          fit: true,
          padding: 30,
          randomize: false,
          componentSpacing: 100,
          nodeRepulsion: function () {
            return 2048;
          },
          edgeElasticity: function () {
            return 32;
          },
          nestingFactor: 8,
          gravity: 0.25,
          numIter: 1000,
          initialTemp: 200,
          coolingFactor: 0.95,
          minTemp: 1.0,
        },
        style: [
          {
            selector: 'node',
            style: {
              'content': 'data(label)',
              'text-valign': 'center',
              'text-halign': 'center',
              'color': 'white',
              'text-wrap': 'wrap', // Enable text wrapping
              'text-max-width': 80, // Adjust the maximum width for wrapping
              'text-outline-width': 2,
              'text-outline-color': '#888',
              'background-color': (ele) => generateColor(ele.data('name')),
              'border-color': 'black',
              'border-width': 2,
            },
          },
          {
            selector: 'edge',
            style: {
              'width': 2,
              'line-color': '#ccc',
              'curve-style': 'bezier',
            },
          },
        ],
      });
    },
  },
};
</script>

<style>
#cy {
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 999;
}
</style>
