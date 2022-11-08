<template>
  <div>
    <v-row align="center">
      <v-col cols="2" md="2">
        <v-textarea  width="20" solo v-model="numerador" name="input-7-4" label="Solo textarea"></v-textarea>
        <hr width="230">
        </hr>

        <v-textarea class="pt-3" solo v-model="denominador" name="input-7-4" label="Solo textarea"></v-textarea>
      </v-col>
      <v-col cols="1" md="1">
        <v-autocomplete v-model="signo" :items="items" outlined dense label="Outlined"></v-autocomplete>
      </v-col>
      <v-col cols="2" md="2">
        <v-textarea solo v-model="numerador2" name="input-7-4" label="Solo textarea"></v-textarea>
        <hr width="230">
        </hr>

        <v-textarea class="pt-3" solo v-model="denominador2" name="input-7-4" label="Solo textarea"></v-textarea>
      </v-col>
      <v-col>
        <template>
        <v-row>
          
       
          <v-col cols="4" md="4">
        <v-textarea solo v-model="instruction" name="input-7-4" label="Instrucciones"></v-textarea>
      <v-textarea solo v-model="help" name="input-7-4" label="Ayuda"></v-textarea>
      </v-col>

          <v-col cols="6" md="6">
            <v-timeline dense clipped>
            <v-slide-x-transition group>
            </v-slide-x-transition>

            <v-timeline-item class="mb-6" hide-dot>
              <span>TODAY</span>
            </v-timeline-item>

            <v-timeline-item v-for="(fraction, i) in fractions" :key="i" fill-dot>
              <v-row justify="space-between">
                <v-col cols="6">
                  <span v-text="fraction.exercise"></span>

                </v-col>
             
                <v-col class="text-right" cols="2">
                  <v-icon small class="mr-2"> mdi-pencil </v-icon>
                  <v-icon small  @click="deleteItem(item)"> mdi-delete </v-icon>
                </v-col>
              </v-row>
            </v-timeline-item>
          </v-timeline>
          
          </v-col>
        </v-row>

   
        </template>
        
      </v-col>
    </v-row>
   
    <v-btn text class="success">
      <span class="mr-2" @click="click">Guardar</span>
    </v-btn>
  </div>
</template>

<script>
// @ is an alias to /src
import Card from "@/components/Card.vue";
import Ranking from "../components/Ranking.vue";

export default {
  name: "dashboard",
  components: {
    Card,
    Ranking,
  },

  data: () => ({

    items: ['+', '-', '÷', 'x'],
    numerador: "",
    denominador: "",
    numerador2: "",
    denominador2: "",
    signo: "",
    instruction: "",
    help: "",
    fractions: "",

    events: [],
    input: null,
    nonce: 0,

  }),

  created() {

    this.getExercises();
  },

  computed: {
    timeline() {
      return this.events.slice().reverse()
    },
  },

  methods: {
    


    click() {

      let fraccion = this.numerador + "/" + this.denominador + "" + this.signo + "" + this.numerador2 + "/" + this.denominador2
      console.log("Se preciono el boton", fraccion)
      let number = 1;

      let exercise = {
        fraccion: fraccion,
        number: number,
        instruction: this.instruction,
        help: this.help
      };
      

      this.axios
        .post("http://localhost:3000/addExercises", exercise,
          {
            params: {
              number: exercise.number,
              exercise: exercise.fraccion,
              instruction: exercise.instruction,
              help: exercise.help
            },
          }
        )
        .then((res) => {
          console.log(res);
          this.getExercises()
        })
        .catch((err) => {
          console.log(err);
        });

    },
    deleteItem(item) {
      //
    },
    async getExercises() {
      const res = await this.axios.get("http://localhost:3000/exercises");
      console.log(res.data)
      this.fractions = res.data
    },

  },
};
</script>
