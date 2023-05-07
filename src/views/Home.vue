<template>
  <div class="home">
    <div class="box">
      <h1>Analisador Léxico</h1>
      <input
        v-model="wordValue"
        type="text"
        placeholder="Insira sua entrada"
        style="font-size: 12px; padding: 7px; margin-bottom: 30px"
      />
      <li
        v-for="(validation, index) in validations"
        :key="index"
        :style="{
          'font-size': '0.8rem',
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
      const regexValidarPrimeiraLetra = /[bcdfglmnprstvxz]/i.test(word);
      this.validations.push({
        text: "A primeira letra deve ser uma consoante válida.",
        status: regexValidarPrimeiraLetra ? true : false,
      });

      const regexValidarSequenciaDaPalavra =
        /^([bcdfglmnprstvxz][aeiou])+[bcdfglmnprstvxz]?\d*$/i.test(
          word.replace(/\s/g, "")
        );
      this.validations.push({
        text: "A palavra deve alternar entre consoantes e vogais podendo haver sequência de números no final.",
        status: regexValidarSequenciaDaPalavra ? true : false,
      });

      const regexExcecaoLetras = /[jwkyçhq]/i.test(word);
      this.validations.push({
        text: 'Não pode conter as letras "j, w, k, y, ç, h e q".',
        status: regexExcecaoLetras ? false : true,
      });

      const regexExcecaoCaracteres = /[/()&%$#@!]/.test(word);
      this.validations.push({
        text: 'Não pode conter caracteres especiais como "/, (, ), &, %, $, #, @ e !".',
        status: regexExcecaoCaracteres ? false : true,
      });

      this.validations.push({
        text: "A palavra deverá ter no máximo 10 caracteres.",
        status: word.length <= 10 ? true : false,
      });

      this.validations.push({
        text: "Palavra começada pela letra Z ou X é uma palavra reservada.",
      });
    },
  },
};
</script>
<style scoped>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50vh;
}
.box {
  display: flex;
  flex-direction: column;
}
</style>
