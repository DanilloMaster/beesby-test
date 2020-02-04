<template>
  <div style="margin: 50px 100px;">
    <div class="d-flex" style="justify-content:center">
      <img
        src="~/assets/img/Logo Beesby.png"
        width="275"
        style="margin-top:30px"
      />
    </div>
    <div style="margin-top:20px">
      <b>Bom teste {{ canditate }}!!  {{new Date().toLocaleDateString()}}</b>
    </div>
    <p style="color:red">{{ Erro }}</p>
    <div
      v-for="ques in questionsTest"
      v-bind:key="ques.id"
      style="margin-top:50px"
    >
      <div v-if="!ques.html">
        <p>{{ ques.text }}</p>
      </div>
      <div v-else>
        <p v-html="ques.text"></p>
      </div>
      <v-radio-group>
        <v-radio
          v-for="awn in ques.awnsers"
          v-bind:key="awn.text"
          @change="checkable(ques, awn)"
          :label="awn.text"
          :disabled="disRadio"
        ></v-radio>
      </v-radio-group>
    </div>
    <p style="color:red">{{ Erro }}</p>
    <div
      v-if="showFinalize"
      class="c-pointer btn-test"
      v-on:click="finalizeTest()"
    >
      FINALIZAR TESTE
    </div>
    <div
      v-if="showFeedBack"
      class="d-flex"
      style="margin-top:40px;margin-bottom:70px; margin: 0 auto; justify-content:space-between"
    >
      <div style="margin-top:25px">
        Gabarito:
        <p v-for="feed in feedback" v-bind:key="feed">{{ feed.text }}</p>
      </div>
      <div class="c-pointer btn-test" v-on:click="print()">
        IMPRIMIR
      </div>
      <div>
        <div style="text-align:center;margin-top :25px">
          Acertos:
        </div>
        <v-progress-circular
          :rotate="rotate"
          :size="size"
          :value="valueProgress"
          :width="width"
          color="light-blue"
          >{{ valueProgressText }}</v-progress-circular
        >
      </div>
    </div>

    <div v-if="generateForm">
      <v-text-field label="Usuário" v-model="user"></v-text-field>
      <v-text-field
        label="Senha"
        v-model="senha"
        type="password"
      ></v-text-field>
      <v-btn @click="generate">GERAR RESULTADO</v-btn>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      questions: [
        {
          id: 1,
          text:
            "Sendo a e b variáveis inteiras em um programa, a expressão lógica NÃO ((a > b) OU (a = b)) é equivalente a:",
          awnsers: [
            { text: "A - (a <= b);", id: 1, check: false },
            { text: "B - (a >= b);", id: 2, check: false },
            { text: "C - (a < b);", id: 3, check: false },
            { text: "D - (a > b)", id: 4, check: false },
            { text: "E - NÃO (a = b).", id: 5, check: false }
          ],
          category: "logic",
          type: "Estagiario",
          correctAwnser: { letter: "C", number: 3 }
        },
        {
          id: 2,
          text:
            "Com relação aos conceitos de linguagens de programação e seus tipos, assinale a opção correta.",
          awnsers: [
            {
              text:
                "A - Em um programa orientado a objetos, cada objeto é uma instância de uma classe na qual estão definidas todas as características de tal objeto. ",
              id: 1,
              check: false
            },
            {
              text:
                "B - Em programação funcional, funções são ditas de ordem superior se os seus argumentos ou resultados são, eles próprios, funções. ",
              id: 2,
              check: false
            },
            {
              text:
                "C - Na programação lógica, os programas são compostos por funções que implementam sentenças da lógica de primeira ordem. ",
              id: 3,
              check: false
            },
            {
              text:
                "D - Em linguagens de programação puramente imperativas, como é o caso da linguagem C++, os programas são vistos como uma sequência de ações.",
              id: 4,
              check: false
            },
            {
              text:
                "E - Todo programa orientado a objetos é composto por classes que estão relacionadas por meio de uma hierarquia de classes. Independente da linguagem orientada a objetos utilizada, a hierarquia pode conter herança múltipla de classes.",
              id: 5,
              check: false
            }
          ],
          category: "logic",
          type: "Estagiario",
          correctAwnser: { letter: "B", number: 2 }
        },
        {
          id: 3,
          text:
            "O comando while utilizado em algoritmos implementa laços com teste antecipado de condições, testando a condição e, sendo ela verdadeira, executando o bloco de comandos.",
          awnsers: [
            { text: "A - Certo. ", id: 1, check: false },
            { text: "B - Errado ", id: 2, check: false }
          ],
          category: "logic",
          type: "Estagiario",
          correctAwnser: { letter: "A", number: 1 }
        },
        {
          id: 4,
          text:
            "Acerca dos conceitos e padrões Java e JavaScript, julgue o item a seguir. A linguagem JavaScript é uma extensão da plataforma Java Standard Edition destinada à criação de códigos interpretados em máquinas virtuais e navegadores web.",
          awnsers: [
            { text: "A - Certo", id: 1, check: false },
            { text: "B - Errado", id: 2, check: false }
          ],
          category: "js",
          type: "Estagiario",
          correctAwnser: { letter: "B", number: 2 }
        },
        {
          id: 5,
          text:
            "Em relação à linguagem de programação javascript, assinale a alternativa correta.",
          awnsers: [
            {
              text: "A - É uma derivação da linguagem JAVA.",
              id: 1,
              check: false
            },
            {
              text: "B - Somente pode ser utilizada em conjunto com CSS.",
              id: 2,
              check: false
            },
            { text: "C - É um padrão ECMA.", id: 3, check: false },
            {
              text: "D - O HTML 5 não suporta o javascript.",
              id: 4,
              check: false
            },
            {
              text:
                "E - É uma linguagem de script para terminais de comandos de sistemas operacionais.",
              id: 5,
              check: false
            }
          ],
          category: "js",
          type: "Estagiario",
          correctAwnser: { letter: "C", number: 3 }
        },
        {
          id: 6,
          text:
            "Para trabalhar com HTML5 e Javascript, é necessário ter uma interface entre a linguagem Javascript e os objetos do HTML5. Para tal, existe uma estrutura que permite essa aplicabilidade e que é amplamente conhecida e utilizada. Qual é o nome dessa estrutura?",
          awnsers: [
            {
              text: "A - TDD – (Test Driven Development).",
              id: 1,
              check: false
            },
            { text: "B - DOM – (Document Object Model).", id: 2, check: false },
            { text: "C - Composer.", id: 3, check: false },
            {
              text: "D - DOI – (Document Objetc Interface).",
              id: 4,
              check: false
            },
            {
              text: "E - ERP – (Enterprise Resource Planning).",
              id: 5,
              check: false
            }
          ],
          category: "js",
          type: "Estagiario",
          correctAwnser: { letter: "C", number: 3 }
        },
        {
          id: 7,
          text:
            'Observe o seguinte código JavaScript:<br> <img src="https://s3.amazonaws.com/qcon-assets-production/images/provas/65221/2aa4d6facc9a00c196a6.png" width="420"/> <br>O resultado apresentado no browser, após a sua execução, é',
          awnsers: [
            { text: "A - 8.", id: 1, check: false },
            { text: "B - 10.", id: 2, check: false },
            { text: "C - 7.", id: 3, check: false },
            { text: "D - 9.", id: 4, check: false },
            { text: "E - 6.", id: 5, check: false }
          ],
          category: "js",
          type: "Estagiario",
          html: true,
          correctAwnser: { letter: "C", number: 3 }
        },
        {
          id: 8,
          text:
            "Na linguagem JavaScript, o operador === (três sinais de igualdade) realiza a comparação apenas do",
          awnsers: [
            { text: "A - tipo dos operandos.", id: 1, check: false },
            { text: "B - conteúdo dos operandos.", id: 2, check: false },
            { text: "C - valor dos operandos.", id: 3, check: false },
            { text: "D - valor lógico dos operandos.", id: 4, check: false },
            { text: "E - valor e do tipo dos operandos.", id: 5, check: false }
          ],
          category: "js",
          type: "Estagiario",
          correctAwnser: { letter: "E", number: 5 }
        },
        {
          id: 9,
          text:
            "No que se refere às tecnologias de desenvolvimento para web, julgue o próximo item. HTML5 é uma ferramenta para desenvolvimento de sistemas para web baseada no padrão REST.",
          awnsers: [
            { text: "A - Certo.", id: 1, check: false },
            { text: "B - Errado.", id: 2, check: false }
          ],
          category: "html",
          type: "Estagiario",
          correctAwnser: { letter: "B", number: 2 }
        },
        {
          id: 10,
          text:
            "Ao desenvolver uma página utilizando a linguagem de marcação HTML, é possível ligarmos a página a um arquivo externo que definirá sua apresentação (aparência). A qual tipo de arquivo o enunciado se refere?",
          awnsers: [
            { text: "A - XML.", id: 1, check: false },
            { text: "B - XML.", id: 2, check: false },
            { text: "C - XHTML.", id: 3, check: false },
            { text: "D - CSS.", id: 4, check: false },
            { text: "E - HTML.", id: 5, check: false }
          ],
          category: "html",
          type: "Estagiario",
          correctAwnser: { letter: "D", number: 4 }
        },
        {
          id: 11,
          text:
            '<LINK REL="STYLESHEET" HREF=" ______ " TYPE="text/css"> Em relação à parte omitida, é correto afirmar que',
          awnsers: [
            {
              text:
                "A - ela indica o nome do arquivo no qual a página elaborada na linguagem HTML se encontra.",
              id: 1,
              check: false
            },
            {
              text:
                "B - ela indica que será criado um arquivo no qual o texto digitado pelo usuário será armazenado.",
              id: 2,
              check: false
            },
            {
              text:
                "C - se trata de um arquivo cuja extensão necessariamente deverá ser txt.",
              id: 3,
              check: false
            },
            {
              text:
                "D - se trata de um arquivo tipo texto e um nome possível para ele poderia ser estilo.css.",
              id: 4,
              check: false
            },
            {
              text:
                "E - se trata de um arquivo com o texto a ser exibido na página que será aberta.",
              id: 5,
              check: false
            }
          ],
          category: "html",
          type: "Estagiario",
          correctAwnser: { letter: "D", number: 4 }
        },
        {
          id: 12,
          text:
            "A propriedade e o valor da regra de estilo CSS que estabelecem a distância de 16px entre o conteúdo de um elemento HTML e a sua borda, no contexto do modelo CSS Box, é:",
          awnsers: [
            { text: "A - margin: 16px;", id: 1, check: false },
            { text: "B - border-width: 16px;", id: 2, check: false },
            { text: "C - word-spacing: 16px;", id: 3, check: false },
            { text: "D - letter-spacing: 16px;", id: 4, check: false },
            { text: "E - padding: 16px;", id: 5, check: false }
          ],
          category: "html",
          type: "Estagiario",
          correctAwnser: { letter: "E", number: 5 }
        },
        {
          id: 13,
          text:
            'Observe o código do programa em C# a seguir: <br> <img src="https://s3.amazonaws.com/qcon-assets-production/images/provas/66118/a644d0163fa63d76dcc1.png"/> <br> Se forem digitados os números 5, 4 e 18, nessa sequência, o resultado exibido será:Se forem digitados os números 5, 4 e 18, nessa sequência, o resultado exibido será:',
          awnsers: [
            { text: "A - 15, 24, 2 6 18", id: 1, check: false },
            { text: "B - 5, 4, 18", id: 2, check: false },
            { text: "C - 15, 24, 1 2 3 6 9 18", id: 3, check: false },
            { text: "D - 10, 16, 1 2 3 6 9 18", id: 4, check: false }
          ],
          html: true,
          category: "html",
          type: "Estagiario",
          correctAwnser: { letter: "D", number: 4 }
        },
        {
          id: 13,
          text:
            'Observe o código do programa em C# a seguir: <br> <img src="https://s3.amazonaws.com/qcon-assets-production/images/provas/66118/a644d0163fa63d76dcc1.png"/> <br> Se forem digitados os números 5, 4 e 18, nessa sequência, o resultado exibido será:Se forem digitados os números 5, 4 e 18, nessa sequência, o resultado exibido será:',
          awnsers: [
            { text: "A - 15, 24, 2 6 18", id: 1, check: false },
            { text: "B - 5, 4, 18", id: 2, check: false },
            { text: "C - 15, 24, 1 2 3 6 9 18", id: 3, check: false },
            { text: "D - 10, 16, 1 2 3 6 9 18", id: 4, check: false }
          ],
          html: true,
          category: "html",
          type: "Estagiario",
          correctAwnser: { letter: "D", number: 4 }
        },
        {
          id: 14,
          text:
            "Num belo domingo de primavera, três homens, as respetivas mulheres e um primo de uma delas – que era viúvo – saíram de carro ao meio-dia para um fazer um piquenique no pinhal. Ao fim de três quilómetros cruzaram-se com dois homens e uma criança cujo carro se tinha avariado no meio da estrada. “Isso é mesmo má sorte”, comentou o homem que ia a conduzir o primeiro carro. Chegaram ao pinhal à uma da tarde, onde encontraram um guarda muito velho na companhia do filho. Estenderam a toalha no chão e começaram a almoçar. Quantas pessoas foram mencionadas nesta história?",
          awnsers: [
            { text: "A - Doze pessoas", id: 1, check: false },
            { text: "B - Dez pessoas", id: 2, check: false },
            { text: "C - Treze pessoas", id: 3, check: false },
            { text: "D - Onze pessoas", id: 4, check: false }
          ],
          category: "logic",
          type: "Estagiario",
          correctAwnser: { letter: "A", number: 1 }
        },
        {
          id: 15,
          text:
            "Olá! Eu sei que não sabe quem sou, mas nós somos da mesma família: o meu pai é irmão da sua irmã. Consegue adivinhar que parente sou eu:",
          awnsers: [
            { text: "A - primo", id: 1, check: false },
            { text: "B - sobrinho", id: 2, check: false },
            { text: "C - filho ", id: 3, check: false },
            { text: "D - tio ", id: 4, check: false }
          ],
          category: "logic",
          type: "Estagiario",
          correctAwnser: { letter: "B", number: 2 }
        },
        {
          id: 16,
          text:
            "Qual das alternativas a baixo corresponde aos 3 pilares da POO (Programação Orientada a Objetos):",
          awnsers: [
            {
              text: "A - Encapsulamento, Herança, Polimorfismo",
              id: 1,
              check: false
            },
            { text: "B - Encapsulamento, Api, Herança", id: 2, check: false },
            {
              text: "C - Encapsulamento, Polimorfismo, Rest",
              id: 3,
              check: false
            },
            {
              text: "D - Encapsulamento, Interface, Herança ",
              id: 4,
              check: false
            }
          ],
          category: "poo",
          type: "Estagiario",
          correctAwnser: { letter: "A", number: 1 }
        },
        {
          id: 17,
          text:
            "Considere que uma classe e uma interface foram criadas (class A e interface IA), na arquiterura defina é requerido que a class A implemente a interface IA, qual das alternativas a baixo corresponde a essa implementação na linguagem C#:",
          awnsers: [
            {
              text: "A - A implements IA",
              id: 1,
              check: false
            },
            { text: "B - A include IA", id: 2, check: false },
            {
              text: "C - A ? IA",
              id: 3,
              check: false
            },
            {
              text: "D - A : IA ",
              id: 4,
              check: false
            }
          ],
          category: "poo",
          type: "Estagiario",
          correctAwnser: { letter: "D", number: 4 }
        },
        {
          id: 18,
          text:
            "Ao se utilizar o CSS (Cascading Style Sheets), cada declaração contém uma propriedade e um valor separados por:",
          awnsers: [
            {
              text: "A - um simples ponto (.)",
              id: 1,
              check: false
            },
            { text: "B - duas barras (//)", id: 2, check: false },
            {
              text: "C - dois pontos (:)",
              id: 3,
              check: false
            },
            {
              text: "D - duas barras verticais (||) ",
              id: 4,
              check: false
            }
          ],
          category: "poo",
          type: "Estagiario",
          correctAwnser: { letter: "C", number: 3 }
        },
        {
          id: 19,
          text:
            "CSS é uma sigla que em inglês significa Cascading Style Sheets. CSS é utilizado em páginas web desenvolvidas com linguagem de marcação, como o XHTML. Há três formas de utilizar CSS nas páginas web. Assinale a alternativa correta em relação a essas três formas.",
          awnsers: [
            {
              text:
                "A - Folha de estilo interna é quando um CSS é escrito dentro do código da página web, aplicando os estilos somente na página em questão.",
              id: 1,
              check: false
            },
            {
              text:
                "B - Folha de estilo em linha é quando um arquivo CSS externo é ligado à página web.",
              id: 2,
              check: false
            },
            {
              text:
                "C - Folha de estilo externa é quando um CSS é descrito em uma tag específica na página web.",
              id: 3,
              check: false
            },
            {
              text:
                "Folha de estilo interna é hierarquicamente mais prioritária que a folha de estilo em linha. ",
              id: 4,
              check: false
            }
          ],
          category: "css",
          type: "Estagiario",
          correctAwnser: { letter: "A", number: 1 }
        },
        {
          id: 20,
          text: "Sobre o framework Vue.js, assinale a alternativa correta.",
          awnsers: [
            {
              text: "A - Vue.js é um framework escrito em Java.",
              id: 1,
              check: false
            },
            {
              text:
                "B - Templates são instâncias reutilizáveis do Vue com um nome.",
              id: 2,
              check: false
            },
            {
              text:
                "C - Vue.js é considerado um framework de backend (server side).",
              id: 3,
              check: false
            },
            {
              text:
                "D - Vue.js possui estruturas que possibilitam utilizar condicionais, mas não possui estruturas de laço. ",
              id: 4,
              check: false
            },
            {
              text:
                "E - Os templates do Vue.js são compostos por HTML válido que podem ser compilados por navegadores compatíveis com as especificações e também por compiladores HTML. ",
              id: 5,
              check: false
            }
          ],
          category: "vue",
          type: "Estagiario",
          correctAwnser: { letter: "E", number: 5 }
        }
      ],
      questionsTest: [],
      feedback: [],
      result: [],
      resultFinal: { correctQuestions: 0, incorrectQuestions: 0 },
      rotate: 0,
      size: 100,
      valueProgress: 0,
      valueProgressText: 0,
      width: 10,
      showFeedBack: false,
      Erro: "",
      disRadio: false,
      canditate: "",
      generateForm: false,
      showFinalize: true,
      user: "",
      senha: ""
    };
  },

  created() {
    this.canditate = this.$route.query.nome;
    for (let i = 1; i <= 10; i++) {
      let num = this.getRandom(1, 10);
      let question = this.questions.find(i => i.id === num);
      if (question) {
        this.questionsTest.push(question);
        this.feedback.push({ text: question.correctAwnser.letter });
      } else {
        i--;
      }
    }

    if (this.questionsTest.length > 10) {
      this.questionsTest.splice(this.questionsTest.length, 1);
    }
  },

  methods: {
    redirect() {
      this.$router.push({ path: "/" });
    },

    generate() {
      if (this.user === "Beesby" && this.senha === "4321") {
        this.showFeedBack = true;
        this.generateForm = false;
        this.showFinalize = false;
        this.Erro = "";
      } else {
        this.Erro = "Usuário ou senha para gerar resultado inválido(s)";
      }
    },

    getRandom(min, max) {
      let num = Math.floor(Math.random() * (max - min + 1)) + min;
      let findQues = this.questionsTest.find(i => i.id === num);
      if (findQues) {
        this.getRandom(min, max);
      } else {
        return num;
      }
    },

    validate() {
      if (this.result.length !== this.questionsTest.length) {
        this.Erro = "Ainda existem perguntas para serem respondidas";
        return false;
      } else {
        this.Erro = "";
        return true;
      }
    },

    print() {
      window.print();
    },

    finalizeTest() {
      if (!this.validate()) return;

      this.showFinalize = false;
      this.generateForm = true;
      this.disRadio = true;
      this.result.forEach(o => {
        if (o.correct) {
          this.resultFinal.correctQuestions += 1;
        } else {
          this.resultFinal.incorrectQuestions += 1;
        }
      });

      if (
        this.questionsTest.length !==
        this.resultFinal.correctQuestions + this.resultFinal.incorrectQuestions
      ) {
        this.valueProgressText = "Ocorreu um erro por favor contatar o suporte";
        return;
      }

      this.valueProgress =
        (this.resultFinal.correctQuestions * 100) / this.questionsTest.length;
      this.valueProgressText = this.resultFinal.correctQuestions;
    },

    checkable(question, awnser) {
      let responseExists = this.result.find(t => t.questionId === question.id);

      if (responseExists) {
        this.result.splice(this.result.indexOf(responseExists), 1);
      }

      let obj = {
        questionId: question.id,
        awnser: awnser,
        correct: awnser.id === question.correctAwnser.number
      };

      this.result.push(obj);
    }
  }
};
</script>