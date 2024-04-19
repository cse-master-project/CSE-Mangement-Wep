<template>
  <q-form class="q-pa-md">
    <q-card>
      <q-card-section>
        <q-select
          v-model="mainCategory"
          :options="mainCategoryOptions"
          label="대분류"
          outlined
          class="q-mb-md"
        />

        <q-select
          v-model="subCategory"
          :options="subCategoryOptions"
          label="소분류"
          outlined
          class="q-mb-md"
        />

        <q-input
          type="textarea"
          v-model="question"
          outlined
          placeholder="문제를 입력해주세요"
          autogrow
          class="q-mb-md"
          maxlength="300"
        />

        <div class="wrapper">
          <div class="left">
            <q-input label="a" v-model="leftOptions.a" outlined />
            <q-input label="b" v-model="leftOptions.b" outlined />
            <q-input label="c" v-model="leftOptions.c" outlined />
          </div>
          <div class="right">
            <q-input label="a`" v-model="rightOptions.a" outlined />
            <q-input label="b`" v-model="rightOptions.b" outlined />
            <q-input label="c`" v-model="rightOptions.c" outlined />
          </div>
        </div>

        <q-input
          type="textarea"
          v-model="answer"
          outlined
          placeholder="답을 입력해주세요 ex) atoa`,btoc`"
          autogrow
          class="q-mb-md"
          maxlength="20"
          style="width: 30%"
        />

        <q-input
          v-model="commentary"
          type="textarea"
          placeholder="해설을 입력해주세요"
          outlined
          autogrow
          class="q-mb-md"
          style="margin: 3% 0"
        />

        <section class="container">
          <label for="file">
            <div class="styled-file-input">
              <div class="attachment-button">🔗 FILE UPLOAD</div>
              <p v-if="fileName" class="attached-file">{{ fileName }}</p>
            </div>
          </label>
          <input type="file" id="file" @change="fileInputHandler" />
        </section>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn
          class="backbtn"
          @click="goBack()"
          style="width: 10%; margin: 3% 0"
          >뒤로</q-btn
        >

        <q-btn class="registerbtn" style="width: 10%; margin: 3% 0"
          >문제 등록</q-btn
        >
      </q-card-actions>
    </q-card>
  </q-form>
</template>

<script setup>
import { ref, defineEmits } from 'vue';
import { QInput } from 'quasar';

const mainCategoryOptions = [
  { label: '과일', value: 'Fruit' },
  { label: 'c언어', value: 'C' },
  { label: '파이썬', value: 'Python' },
  { label: '자료구조', value: 'Data structure' },
];

const subCategoryOptions = [
  { label: '색', value: 'Color' },
  { label: '스택', value: 'Stack' },
  { label: '큐', value: 'Queue' },
  { label: '그래프', value: 'Graph' },
];

const mainCategory = ref('');
const subCategory = ref('');
const question = ref('');
const leftOptions = ref({
  a: '',
  b: '',
  c: '',
});

const rightOptions = ref({
  a: '',
  b: '',
  c: '',
});
const commentary = ref('');
const emits = defineEmits(['change-quiz-type']);

const goBack = () => {
  emits('change-quiz-type', '');
};
</script>

<style scoped lang="scss">
.wrapper {
  display: flex;
  margin: 30px 0;
}

.left,
.right {
  display: flex;
  flex-direction: column;
  margin: 10px auto;
  width: 300px;
}
.left > *,
.right > * {
  margin: 10px 0;
}
.answer {
  height: 30px;
}
</style>
