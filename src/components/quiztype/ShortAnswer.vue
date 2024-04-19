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
          v-model="question"
          type="textarea"
          autogrow
          outlined
          placeholder="문제를 입력해주세요"
          class="textbox"
          maxlength="300"
        />
        <q-input
          v-model="answer"
          type="textarea"
          autogrow
          outlined
          placeholder="답을 입력해주세요"
          class="textbox"
          style="margin: 3% 0; width: 30%"
        />
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
const answer = ref('');
const commentary = ref('');
const emits = defineEmits(['change-quiz-type']);

const goBack = () => {
  emits('change-quiz-type', '');
};
</script>

<style scoped lang="scss">
.textbox {
  font-family: 'Arial', sans-serif;
}
</style>
