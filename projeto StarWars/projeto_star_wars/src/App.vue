<template>
  <v-app>
    <div :class="theme">
      <v-app-bar>Projeto StarWars</v-app-bar>
      <v-main>
        <v-container>
          <v-row>
            <v-tabs v-model="tab" background-color="deep-purple accent-4" dark>
              <v-tab value="personagens">Personagens</v-tab>
              <v-tab value="planetas">Planetas</v-tab>
              <v-tab value="api">API</v-tab>
            </v-tabs>

            <v-card-text>
              <v-tabs-window v-model="tab">
                <v-tabs-window-item value="personagens">
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
                      v-for="card in filteredPersonagensCards"
                      :key="card.title"
                      cols="12"
                      sm="6"
                      md="4"
                      lg="3"
                    >
                      <v-card>
                        <v-img :src="card.image" class="hover-image">
                          <v-card-title style="text-align: left">{{
                            card.title
                          }}</v-card-title>
                        </v-img>
                        <v-card-subtitle>{{ card.subtitle }}</v-card-subtitle>
                        <v-card-text style="text-align: center">
                          <div style="text-align: center">
                            {{ card.description.substring(0, 100) }}...
                          </div>
                        </v-card-text>
                        <v-card-actions>
                          <v-btn
                            variant="outlined"
                            color="primary"
                            @click="openDialog(card)"
                            >Ver mais</v-btn
                          >
                        </v-card-actions>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-tabs-window-item>

                <v-tabs-window-item value="planetas">
                  <Planetas :search="search" />
                </v-tabs-window-item>

                <api />
              </v-tabs-window>
            </v-card-text>
          </v-row>

        

          <v-dialog v-model="dialog" max-width="600px">
            <v-card>
              <v-card-title>{{ selectedItem.title }}</v-card-title>
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
        </v-container>
      </v-main>
    </div>
  </v-app>
</template>

<script>
import Planetas from "./components/planetas.vue";
import Api from "./components/api.vue";
export default {
  components: {
    Api, // Use a letra maiúscula
    Planetas,
  },
  data() {
    return {
      resourcePersonagens: [],
      resourcePlanetas: [],
      search: "",
      loading: false,
      tab: "personagens",
      dialog: false,
      selectedItem: {},
      personagensCards: [
        {
          title: "C-3PO",
          subtitle: "Tradutor de Línguas e Aventureiro Nervoso",
          description:
            " Andróide de protocolo com uma personalidade ansiosa e um vasto conhecimento de idiomas e culturas galácticas. C-3PO é frequentemente o alívio cômico das histórias, expressando preocupação e desespero em momentos críticos. Embora não possua habilidades de combate, sua capacidade de traduzir e mediar interações é essencial para a equipe.",
          image: "../images/c-3po.jfif",
        },
        {
          title: "Chewbacca",
          subtitle: "A Força e a Lealdade em Forma de Wookiee",
          description:
            "O fiel co-piloto da Millennium Falcon e amigo de Han Solo, Chewbacca é um Wookiee de grande força e habilidades de combate. Ele se destaca por sua lealdade incondicional e bravura, além de seu papel crucial em diversas missões da Rebelião. Sua comunicação através de rosnados e grunhidos, junto com sua conexão emocional com Han, o torna um personagem memorável e amado.",
          image: "../images/Chewbacca.jfif",
        },
        {
          title: "Darth Vader",
          subtitle: "O Lado Sombrio da Força",
          description:
            "Originalmente Anakin Skywalker, um Jedi promissor, ele sucumbe ao lado sombrio e se torna o temido Darth Vader, braço direito do Imperador. Com habilidades de combate impressionantes e um domínio aterrador da Força, ele é um símbolo do mal na galáxia. A busca por redenção e a luta interna entre o bem e o mal são centrais em sua história.",
          image: "../images/dart.jfif",
        },
        {
          title: "Finn",
          subtitle: "O Despertar do Coragem",
          description:
            "Finn, originalmente um stormtrooper chamado FN-2187, abandona o Primeiro Império em busca de uma nova identidade e propósito. Sua jornada o leva a se unir à Resistência, onde se torna um aliado crucial na luta contra a opressão. Com um forte senso de justiça e coragem, Finn evolui de um soldado programado para um herói que luta por aquilo em que acredita.",
          image: "../images/finn.jfif",
        },
        {
          title: "Han Solo",
          subtitle: "O Contrabandista com Coração de Herói",
          description:
            "Um contrabandista carismático e audacioso, Han é o piloto da Millennium Falcon. Conhecido por seu senso de humor e coragem, ele se junta à Rebelião inicialmente por interesse próprio, mas se torna um herói leal e um amigo inabalável. Seu relacionamento com a Princesa Leia e sua rivalidade com o Império mostram seu crescimento como personagem.",
          image: "../images/han_solo.jfif",
        },
        {
          title: "Luke Skywalker",
          subtitle: "O Último Jedi em Busca da Esperança",
          description:
            "Jovem agricultor de Tatooine, Luke se torna um Jedi heroico ao descobrir seu destino e seu potencial na Força. Motivado pela perda de seus tios e pelo desejo de combater o Império, ele embarca em uma jornada épica ao lado de aliados como Han Solo, Leia Organa e Obi-Wan Kenobi. Ao longo do caminho, Luke enfrenta desafios imensos, incluindo confrontos com Darth Vader e a busca por respostas sobre sua linhagem. Com o treinamento de Yoda em Dagobah, ele aprende a dominar suas habilidades e a importância do sacrifício, culminando em sua luta para redimir seu pai e restaurar a esperança na galáxia. Seu crescimento de um jovem sonhador para um líder destemido é uma história inspiradora de coragem, amizade e a luta entre o bem e o mal.",
          image: "../images/Luke_skywalker.jfif",
        },
        {
          title: " Leia Organa",
          subtitle: "A Princesa e Líder Rebelde",
          description:
            "Leia Organa, filha de Anakin Skywalker e Padmé Amidala, é uma líder corajosa e destemida da Aliança Rebelde. Conhecida por sua inteligência e habilidades diplomáticas, ela é uma combatente feroz contra o Império. Sua força e determinação a tornam uma figura central na luta pela liberdade, e sua jornada inclui enfrentar desafios pessoais enquanto busca salvar a galáxia.",
          image: "../images/leia.jfif",
        },
        {
          title: "Obi-Wan Kenobi",
          subtitle: "O Mentor Sábio e Protetor da Galáxia",
          description:
            "Um dos Jedi mais respeitados da Ordem, Obi-Wan é conhecido por sua sabedoria, habilidades em combate e devoção à causa Jedi. Ele treinou Anakin Skywalker e, após a queda de seu aprendiz, se tornou mentor de Luke. Seu equilíbrio entre compaixão e firmeza o torna um dos pilares da resistência contra o lado sombrio.",
          image: "../images/obi3.jfif",
        },
        {
          title: "Poe Dameron",
          subtitle: "O Melhor Piloto da Resistência",
          description:
            "Poe Dameron é um piloto excepcional da Resistência, conhecido por suas habilidades de combate aéreo e sua lealdade. Com um espírito destemido e uma personalidade carismática, ele é um dos principais combatentes na luta contra o Primeiro Império. Amigo de Finn e Leia, Poe se destaca em missões arriscadas, sempre pronto para enfrentar os desafios que surgem na batalha pela liberdade.",
          image: "../images/poe.jfif",
        },
        {
          title: "R2-D2",
          subtitle: "O Droid Valente que Sempre Salva o Dia",
          description:
            "Um robô astromecânico icônico, R2-D2 é conhecido por sua inteligência, bravura e habilidades técnicas excepcionais. Ele desempenha papéis vitais em muitas missões, consertando naves e ajudando a salvar os heróis em diversas situações. Seu comportamento leal e sua capacidade de se comunicar de forma única o tornam um personagem querido entre os fãs.",
          image: "../images/r2-d2.jfif",
        },

        {
          title: "Rey",
          subtitle: "A Última Esperança",
          description:
            " Rey é uma jovem scavenger do desértico planeta Jakku que descobre seu potencial na Força. Inicialmente em busca de seus próprios passados, ela se junta à Resistência e se torna uma heroína na luta contra o Primeiro Império. Treinada por figuras como Leia e Luke, Rey busca não apenas entender suas habilidades, mas também encontrar seu lugar em uma galáxia marcada pela luta entre luz e escuridão.",
          image: "../images/Rey.jfif",
        },
        {
          title: "Yoda",
          subtitle: "Sabedoria em Forma Pequena",
          description:
            "Um dos Jedi mais poderosos da galáxia, Yoda é famoso por sua pequena estatura, mas imenso conhecimento e habilidades na Força. Ele treina Jedi por séculos e é um símbolo de sabedoria e resistência. Seu jeito peculiar de falar e seu profundo entendimento da Força o tornam um mentor reverenciado, sempre em busca de manter o equilíbrio.",
          image: "../images/yoda.jfif",
        },
      ],
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

<style>
.hover-image {
  transition: transform 0.3s ease;
}
.hover-image:hover {
  transform: scale(1.1); /* Aumenta a imagem em 10% */
}
</style>
