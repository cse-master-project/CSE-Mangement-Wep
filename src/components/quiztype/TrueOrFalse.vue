<template>
  <q-form class="q-pa-md">
    <q-card>
      <q-card-section
        ><q-select
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
          v-model="question"
          type="textarea"
          autogrow
          outlined
          placeholder="문제를 입력해주세요"
          class="textbox"
          maxlength="300"
        />

        <q-option-group v-model="selectedAnswer" :options="options" inline />

        <q-input
          v-model="commentary"
          type="textarea"
          placeholder="해설을 입력해주세요"
          outlined
          autogrow
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
import { QInput, QOptionGroup } from 'quasar';

const options = [
  { label: 'O', value: 'O' },
  { label: 'X', value: 'X' },
];

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
const selectedAnswer = ref(null);
const commentary = ref('');
const emits = defineEmits(['change-quiz-type']);

const goBack = () => {
  emits('change-quiz-type', '');
};
//첨부파일명 표시
const fileName = ref('');
const fileInputHandler = event => {
  const files = event.target && event.target.files;
  if (files && files[0]) {
    fileName.value = event.target.files[0].name;
  }
};
</script>

<style lang="scss" scoped></style>
