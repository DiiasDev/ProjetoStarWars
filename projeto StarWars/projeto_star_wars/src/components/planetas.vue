<template>
  <div class="content_p">
    <v-row cols="12" class="mt-2">
      <v-card class="mx-auto" color="surface-light" width="1000px">
        <v-card-text>
          <v-text-field
            v-model="search"
            :loading="loading"
            append-inner-icon="mdi-magnify"
            density="compact"
            label="Pesquisar"
            variant="solo"
            hide-details
            single-line
            @click:append-inner="onClick"
          ></v-text-field>
        </v-card-text>
      </v-card>
    </v-row>
    <v-row>
      <v-col
        v-for="planeta in filteredPlanetaCards"
        :key="planeta.title"
        cols="12"
        sm="6"
        md="4"
        lg="3"
      >
        <v-card>
          <v-img :src="planeta.image" class="hover-image">
            <v-card-title style="text-align: left">{{ planeta.title }}</v-card-title>
          </v-img>
          <v-card-subtitle>{{ planeta.subtitle }}</v-card-subtitle>
          <v-card-text>
            <div>{{ planeta.description.substring(0, 100) }}...</div>
          </v-card-text>
          <v-card-actions>
            <v-btn
              variant="outlined"
              color="primary"
              @click="openDialog(planeta)"
            >Ver mais</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <!-- Modal para ver mais detalhes -->
    <v-dialog v-model="dialog" max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">{{ selectedItem.title }}</span>
        </v-card-title>
        <v-card-subtitle>{{ selectedItem.subtitle }}</v-card-subtitle>
        <v-card-text>
          <v-img :src="selectedItem.image" class="hover-image"></v-img>
          <div>{{ selectedItem.description }}</div>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" @click="dialog = false">Fechar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: "Planetas",
  data() {
    return {
      resourcePersonagens: [],
      resourcePlanetas: [],
      search: "",
      loading: false,
      tab: "personagens",
      dialog: false,
      selectedItem: {},
      planetaCards: [
        {
          title: "Coruscant",
          subtitle: "A Cidade do Conhecimento",
          description:
            "O centro político da galáxia, Coruscant é uma cidade-planetária vibrante e densamente povoada. Sua superfície é coberta por arranha-céus e infraestrutura urbana, abrigando o Senado Galáctico e a Ordem Jedi. O planeta representa a civilização e a cultura galáctica, mas também é o local onde a corrupção e as intrigas políticas se desenrolam.",
          image: "../images/Coruscant.jfif",
        },
        {
          title: "Dagobah",
          subtitle: "O Refúgio da Sabedoria",
          description:
            "Um mundo pantanoso e isolado, Dagobah é envolto em mistério e natureza selvagem. É aqui que Luke Skywalker busca treinamento sob a orientação de Yoda. O ambiente úmido e a densa vegetação refletem os desafios internos que Luke deve enfrentar enquanto descobre seu verdadeiro potencial como Jedi.",
          image: "../images/Dagobah.jfif",
        },
        {
          title: "Endor",
          subtitle: "O Lar dos Ewoks",
          description:
            "Um planeta coberto por florestas exuberantes, Endor é famoso por suas vastas árvores e pela presença dos Ewoks, pequenas criaturas nativas que ajudam os rebeldes na luta contra o Império. O planeta é o cenário da batalha decisiva na Retorno de Jedi, onde a união dos Ewoks e dos aliados da Rebelião resulta na queda do Imperador e na vitória da Aliança.",
          image: "../images/Endor.jfif",
        },
        {
          title: "Hoth",
          subtitle: "A Gelo Frio da Rebelião",
          description:
            "Um planeta gelado e inóspito, Hoth é conhecido por suas tempestades de neve e vastas extensões de gelo. É o local da base rebelde Echo, onde a Aliança Rebelde se esconde do Império. A batalha climática entre os rebeldes e as forças imperiais aqui destaca a resistência e a bravura dos que lutam pela liberdade em condições extremas.",
          image: "../images/Hoth.jfif",
        },
        {
          title: "Jakku",
          subtitle: " O Deserto Remoto",
          description:
            "Um mundo desértico e isolado, Jakku é um lugar marcado por antigas batalhas e restos da guerra entre a Aliança Rebelde e o Império. A paisagem é dominada por areias e destroços de naves, refletindo o impacto das lutas passadas. Rey, uma das protagonistas da nova trilogia, cresce neste planeta, e suas origens estão intimamente ligadas ao legado de conflito e esperança da galáxia.",
          image: "../images/Jakku.jfif",
        },
        {
          title: "Mustafar",
          subtitle: "O Inferno em Chamas",
          description:
            "Um mundo vulcânico e ardente, Mustafar é marcado por lava incandescente e erupções constantes. É um local sombrio na história de Star Wars, sendo onde Anakin Skywalker se transforma em Darth Vader após uma batalha trágica com Obi-Wan Kenobi. O planeta simboliza a luta entre luz e escuridão e as consequências da corrupção e da ambição.",
          image: "../images/Mustafar.jfif",
        },
        {
          title: "Naboo",
          subtitle: "A Beleza da Serenidade",
          description:
            "Um planeta exuberante, conhecido por suas paisagens deslumbrantes, lagos serenos e arquitetura elegante. É o lar dos Gungans e dos humanos, e serve como um importante centro cultural e político na galáxia. A beleza de Naboo contrasta com os eventos trágicos da história, incluindo a ascensão de Palpatine e os conflitos que afetam a galáxia.",
          image: "../images/Naboo.jfif",
        },
        {
          title: "Tatooine",
          subtitle: "O Deserto dos Fremen",
          description:
            "Um desértico mundo em sua maioria árido, Tatooine é conhecido por suas vastas dunas de areia e dois sóis escaldantes. Lar de muitos habitantes, incluindo os Tusken Raiders e os Jawas, o planeta é um centro de contrabando e comércio. Luke Skywalker cresceu aqui, e suas aventuras começam neste lugar remoto, onde a força da galáxia parece distante.",
          image: "../images/Tatooine.jfif",
        },
      ],
    };
  },
  computed: {
    filteredPersonagensCards() {
      return this.personagensCards.filter((card) =>
        card.title.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredPlanetaCards() {
      return this.planetaCards.filter((planet) =>
        planet.title.toLowerCase().includes(this.search.toLowerCase())
      );
    },
  },
  methods: {
    openDialog(item) {
      this.selectedItem = item;
      this.dialog = true;
    },
    onClick() {
      this.loading = true;
      setTimeout(() => {
        this.loading = false;
      }, 2000);
    },
    async get_personagens() {
      const url = "https://swapi.dev/api/people";

      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`Response status: ${response.status}`);
        }

        const json = await response.json();
        console.log(json);
        this.resourcePersonagens = json.results;
      } catch (error) {
        console.error(error.message);
      }
    },

    async get_planetas() {
      const url = "https://swapi.dev/api/planets";

      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`Response status: ${response.status}`);
        }

        const json = await response.json();
        console.log(json);
        this.resourcePlanetas = json.results;
      } catch (error) {
        console.error(error.message);
      }
    },
  },
  async mounted() {
    this.get_personagens();
    this.get_planetas();
  },
};
</script>