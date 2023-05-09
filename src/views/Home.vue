<template>
  <div class="home">
    <div class="box">
      <h1>Analisador Léxico</h1>
      <input
        v-model="wordValue"
        type="text"
        placeholder="Insira sua entrada"
        style="padding: 10px; margin-bottom: 20px"
      />
      <h2
        v-show="validations.length > 0"
        :style="{
          margin: '20px auto 20px auto',
          color: validations.find((val) => val.status === false)
            ? 'red'
            : 'green',
        }"
      >
        {{
          validations.find((val) => val.status === false)
            ? `Palavra: "${wordValue}" é inválida`
            : `Palavra: "${wordValue}" é válida`
        }}
      </h2>
      <li
        v-for="(validation, index) in validations"
        :key="index"
        :style="{
          'font-size': '18px',
          color:
            validation.status === true
              ? 'green'
              : validation.status === false
              ? 'red'
              : '',
        }"
      >
        {{ validation.text }}
      </li>
    </div>
  </div>
</template>
<script>
export default {
  name: "Home",
  data: () => ({
    wordValue: "",
    validations: [],
  }),
  mounted() {
    this.validations = [];

    console.log(`
Seja bem-vindo ao nosso projeto!
Esse projeto foi desenvolvido por:
- Alexandre Hideki
- Adriana Pecorelli
- Wesley Rubens
    `);
  },
  watch: {
    wordValue(word) {
      this.validations = [];
      this.verifyWord(word);
    },
  },
  methods: {
    verifyWord(word) {
      this.validations.push(
        {
          //? regexValidarPrimeiraLetra
          text: "A primeira letra deve ser uma consoante válida.",
          status: /^[bcdfglmnprstvxz]/i.test(word) ? true : false,
        },
        {
          //? regexValidarSequenciaDaPalavra
          text: "A palavra deve alternar entre consoantes e vogais podendo haver um algarismo numérico ao final.",
          status: /^([bcdfglmnprstvxz][aeiou])+[bcdfglmnprstvxz]?\d?$/i.test(
            word.replace(/\s/g, "")
          )
            ? true
            : false,
        },
        {
          //? regexExcecaoLetras
          text: 'Não pode conter as letras "j, w, k, y, ç, h e q".',
          status: /[jwkyçhq]/i.test(word) ? false : true,
        },
        {
          //? regexExcecaoCaracteres
          text: 'Não pode conter caracteres especiais como "/, (, ), &, %, $, #, @ e !".',
          status: /[/()&%$#@!]/.test(word) ? false : true,
        },
        {
          text: "A palavra deverá ter no máximo 10 caracteres.",
          status: word.length <= 10 ? true : false,
        },
        ...(/^[zx]/i.test(word)
          ? [
              {
                text: "Palavra começada com a letra Z ou X é uma palavra reservada.",
                status: false,
              },
            ]
          : [])
      );
    },
  },
};
</script>
<style scoped>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
}
.box {
  display: flex;
  flex-direction: column;
}
</style>
