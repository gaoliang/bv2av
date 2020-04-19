<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img :src="require('../assets/bilibili.png')" class="my-3" contain height="100" />
      </v-col>

      <v-col class="mb-5" cols="12">
        <div>
          <v-form ref="form" v-model="valid">
            <v-row justify="center">
              <v-col cols="12" md="4">
                <v-text-field label="BV" :rules="rules" hide-details="auto" v-model="bv"></v-text-field>
              </v-col>
              <v-col cols="12" md="4">
                <v-text-field label="AV" v-model="av" append-icon="open-in-new"></v-text-field>
              </v-col>
            </v-row>
          </v-form>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
// from https://www.zhihu.com/question/381784377/answer/1099438784
const map = [..."fZodR9XQDSUm21yCkr6zBqiveYah8bt4xsWpHnJE7jL5VG3guMTKNPAwcF"];
const s = [11, 10, 3, 8, 4, 6];
const xor = 177451812;
const add = 8728348608;
const pattern = /[Bb][Vv][fZodR9XQDSUm21yCkr6zBqiveYah8bt4xsWpHnJE7jL5VG3guMTKNPAwcF]{10}/;

const bv2av = bv => {
  if (!bv) {
    return "";
  }
  let result = 0;
  let i = 0;
  while (i < 6) {
    result += map.indexOf(bv[s[i]]) * 58 ** i;
    i += 1;
  }
  return `av${(result - add) ^ xor}`;
};

export default {
  name: "HelloWorld",

  data: () => ({
    valid: false,
    bv: "",
    rules: [
      value => !!value || "？？？",
      value => {
        return pattern.test(value) || "不太合适吧";
      }
    ]
  }),
  computed: {
    av: function() {
      if (!this.valid) {
        return "";
      }
      return bv2av(this.bv);
    }
  }
};
</script>
