<template>
  <v-container>
    <v-layout text-xs-center wrap>
      <v-flex mb-5 xs12>
        <h2 class="headline font-weight-bold mb-3">Возможные ходы коня</h2>
        <v-layout justify-center>
          <v-card width="300px">
            <v-text-field class="text-input" v-model="coordinate" label="Координата:" required></v-text-field>
            <br>
            <v-btn @click="getInfo" color="info">Узнать</v-btn>
          </v-card>
          <v-card width="300px">
            <!-- Тут, просто выводим доску -->
            <ul>
              <li v-for="(item, index) in chessField" :key="index">
                <ul>
                  <li class="inner-li" v-for="(elem, key) in item" :key="key">{{elem}}{{index+1}}</li>
                </ul>
              </li>
            </ul>
          </v-card>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    coordinate: "E4",
    alphabet: ["A", "B", "C", "D", "E", "F", "G", "H"],
    chessField: []
  }),
  mounted() {
    let alphabet = this.alphabet;
    let field = new Array();
    // генерируем двумерный массив шахматной доски
    for (let i = 0; i < 8; i++) {
      field[i] = [];
      for (let j = 0; j < 8; j++) {
        field[i][j] = alphabet[j];
      }
    }
    //клаем полученный масив в data
    this.chessField = field;
  },
  methods: {
    getInfo() {
      //основная функция поиска нужных координат
      this.coordinate = this.coordinate.toUpperCase();
      let alphabet = this.alphabet;
      let pointArr = this.coordinate.split("");
      let alertedArray = new Array();
      let magicArray = [
        [-2, -1],
        [-1, -2],
        [1, 2],
        [2, 1],
        [-1, 2],
        [2, -1],
        [-2, 1],
        [1, -2]
      ];
      let numberOfCell = this.chessField[pointArr[1] - 1]; //номер клетки, напр: 1,2,3...
      let letterOfCell = alphabet.indexOf(pointArr[0]); //буквенная часть названия клетки, напр: A,B,C..
      for (let i = 0; i < 8; i++) {
        try {
          if( +pointArr[1] + magicArray[i][1] >= 1 && +pointArr[1] + magicArray[i][1] <= 8)
            alertedArray.push(numberOfCell[letterOfCell + magicArray[i][0]].concat(+pointArr[1] + magicArray[i][1])) //пушим в массив искомые клетки
        } catch (err) {
        //  console.log('Случился не успех')
        }

      }
        alert(`Конь может сходить на следущие поля: ${alertedArray}`)
     
    }
  }
};
</script>

<style>
.text-input {
  margin: 5px 20px;
}
li {
  list-style: none;
}
ul {
  padding-left: 0;
}
.inner-li {
  display: inline-block;
  padding: 5px 7px;
}
</style>
