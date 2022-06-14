<template>
  <v-app>
    <v-app-bar app
               color="#0088b7"
               dark
               class="white--text body-1 font-weight-bold">
      <v-row>
        <v-col align-self="center">
          Trabalho VVT - Testes ESlint em Vue.js
        </v-col>
        <v-col align-self="center">
          <v-row justify="end">
            <v-img src="@/assets/ufms.png"
                   max-height="62"
                   max-width="62"
                   class="mr-3">
            </v-img>
          </v-row>
        </v-col>
      </v-row>
    </v-app-bar>

    <v-main>
      <v-row justify="center">
        <v-card width="80%"
                class="pa-3 mt-10">
          <v-row>
            <v-col cols="7">
              <v-card-title>
                Média de Notas Bimestrais
              </v-card-title>
              <v-text-field label="Nome Completo"
                            class="campo-nome" v-model="campos.nome"
                            outlined>
              </v-text-field>
              <v-text-field label="Matéria"
                            class="campo-disciplina" v-model="campos.disciplina"
                            outlined>
              </v-text-field>
              <v-row>
                <v-col class="pb-0">
                  <v-text-field label="Nota 1º Bimestre"
                                class="campo-nota" v-model="campos.nota1" type="number"
                                outlined>
                  </v-text-field>
                </v-col>
                <v-col class="pb-0">
                  <v-text-field label="Nota 2º Bimestre"
                                class="campo-nota" v-model="campos.nota2" type="number"
                                outlined>
                  </v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="pb-0 pt-0">
                  <v-text-field label="Nota 3º Bimestre"
                                class="campo-nota" v-model="campos.nota3" type="number"
                                outlined>
                  </v-text-field>
                </v-col>
                <v-col class="pb-0 pt-0">
                  <v-text-field label="Nota 4º Bimestre"
                                class="campo-nota" v-model="campos.nota4" type="number"
                                outlined>
                  </v-text-field>
                </v-col>
              </v-row>
            </v-col>
            <v-col cols="5">
              <v-carousel height="300"
                          hide-delimiter-background
                          show-arrows-on-hover>
                <template v-slot:prev="{ on, attrs }">
                  <v-btn color="grey"
                         v-bind="attrs"
                         v-on="on"
                         fab
                         dark
                         small
                         :style="revisao.length <= 1 ? 'display: none !important;' : '' ">
                    <v-icon>
                      mdi-arrow-left
                    </v-icon>
                  </v-btn>
                </template>
                <template v-slot:next="{ on, attrs }">
                  <v-btn color="grey"
                         v-bind="attrs"
                         v-on="on"
                         fab
                         dark
                         small
                         :style="revisao.length <= 1 ? 'display: none !important;' : '' "
                  >
                    <v-icon>
                      mdi-arrow-right
                    </v-icon>
                  </v-btn>
                </template>

                <v-carousel-item v-for="item in revisao"
                                 :key="item.nome">
                  <v-sheet color="white" height="80%" width="260">
                    <div class="ml-2 mt-16">
                      <h4 class="mb-4 text-truncate">{{ item.nome }}</h4>
                      <h5 class="mb-1"> {{ item.disciplina }}</h5>
                      <p class="mb-1">Media: {{ item.media }}</p>

                      <p class="mb-1">
                        Condição: <span :class="condicao(item.media) ? 'green--text' : 'red--text'">{{ condicao(item.media) ? "Aprovado" : "Reprovado"}}</span>
                      </p>

                      <v-row class="mt-7">
                        <v-col>
                          <p class="mb-1">Nota 1º: {{ item.nota1 }}</p>
                        </v-col>
                        <v-col>
                          <p class="mb-1">Nota 2º: {{ item.nota2 }}</p>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col>
                          <p class="mb-1">Nota 3º: {{ item.nota3 }}</p>
                        </v-col>
                        <v-col>
                          <p class="mb-1">Nota 4º: {{ item.nota4 }}</p>
                        </v-col>
                      </v-row>
                    </div>
                  </v-sheet>
                </v-carousel-item>
              </v-carousel>

              <v-btn color="green"
                     class="white--text mt-7"
                     width="96%"
                     height="50px"
                     @click="registrar()">
                Registrar
              </v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      teste: null,
      campos: {
        nome: null,
        disciplina: null,
        nota1: null,
        nota2: null,
        nota3: null,
        nota4: null
      },
      revisao: []
    }
  },
  methods: {
    registrar() {
      let valorItem = {
        nome: this.campos.nome,
        disciplina: this.campos.disciplina,
        media: this._calculaMedia(),
        nota1: this.notaFormatador(this.campos.nota1),
        nota2: this.notaFormatador(this.campos.nota2),
        nota3: this.notaFormatador(this.campos.nota3),
        nota4: this.notaFormatador(this.campos.nota4),
      }
      this.revisao.push(valorItem)
      this._limpar()
    },
    condicao(media) {
      if (media >= 6){
        return true
      } else {
        return false
      }
    },
    notaFormatador(nota) {
      return parseFloat(nota).toFixed(2)
    },
    // estadoBotao() {
    //   if (this.campos.nome != null && this.campos.disciplina != null && this.campos.nota1!= null && this.campos.nota2 != null && this.campos.nota3 != null && this.campos.nota4 != null){
    //     return true
    //   } else {
    //     return false
    //   }
    // },
    _calculaMedia() {
      let media = (parseFloat(this.campos.nota1) + parseFloat(this.campos.nota2) + parseFloat(this.campos.nota3) + parseFloat(this.campos.nota4)) / 4
      return media.toFixed(2)
    },
    _limpar() {
      this.campos = {
        nome: null,
        disciplina: null,
        nota1: null,
        nota2: null,
        nota3: null,
        nota4: null
      }
    }
  }
}
</script>

<style>
.campo-nome {
  width: 100%;
  margin-left: 15px !important;
}

.campo-disciplina {
  width: 100%;
  margin-left: 15px !important;
}

.campo-nota {
  width: 100%;
  margin-left: 15px !important;
}
</style>