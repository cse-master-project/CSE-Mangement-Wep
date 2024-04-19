<template>
  <q-form class="q-pa-md">
    <q-card>
      <q-card-section>
        <!-- 대분류, 소분류는 만든거 이용-->
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
          outlined
          rows="4"
          placeholder="문제를 입력해주세요"
          maxlength="300"
          class="q-mb-md"
        />

        <!-- 보기 입력 4개-->
        <div v-for="index in 4" :key="index" class="choice-container">
          <q-input
            v-model="choices[index - 1].description"
            type="textarea"
            :label="'보기 ' + index"
            outlined
            autogrow
            style="margin: 10px 0"
            class="q-mb-md"
          />
        </div>
        <q-select
          v-model="answer"
          :options="choiceOptions"
          label="정답"
          outlined
          style="width: 10%"
          class="q-mb-md"
        />
        <q-input
          v-model="commentary"
          type="textarea"
          placeholder="해설을 입력해주세요"
          outlined
          autogrow
          style="margin: 3% 0"
        />
      </q-card-section>

      <!-- 첨부파일 입니다.-->
      <q-card-section class="container">
        <label for="file">
          <div class="styled-file-input">
            <div class="attachment-button">🔗 FILE UPLOAD</div>
            <p v-if="fileName" class="attached-file">{{ fileName }}</p>
          </div>
        </label>
        <input type="file" id="file" @change="fileInputHandler" />
      </q-card-section>
      <q-card-actions align="right">
        <q-btn
          class="backbtn"
          @click="goBack()"
          style="width: 10%; margin: 3% 0"
          >뒤로</q-btn
        >
        <q-btn
          class="registerbtn"
          @click="submitQuiz"
          style="width: 10%; margin: 3% 0"
          >문제 등록</q-btn
        >
      </q-card-actions>
    </q-card>
  </q-form>
</template>

<script setup>
import { ref, defineEmits, watch } from 'vue';

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

const choices = ref([
  { value: '1', label: '' },
  { value: '2', label: '' },
  { value: '3', label: '' },
  { value: '4', label: '' },
]);

const mainCategory = ref(''); //대분류
const subCategory = ref(''); //소분류
const choiceOptions = ref([]); //선지

const question = ref(''); //문제
const answer = ref(''); //답
const commentary = ref(''); //해설

watch(
  choices,
  newChoices => {
    choiceOptions.value = newChoices.map((choice, index) => ({
      label: `${index + 1}`,
      value: choice.value,
    }));
  },
  { deep: true, immediate: true },
);

//첨부파일명 표시
const fileName = ref('');
const fileInputHandler = event => {
  const files = event.target && event.target.files;
  if (files && files[0]) {
    fileName.value = event.target.files[0].name;
  }
};
//뒤로가기
const emits = defineEmits(['change-quiz-type']);
const goBack = () => {
  emits('change-quiz-type', '');
};

const submitQuiz = () => {
  // 여기에 문제 제출 로직을 구현합니다.
  console.log('제출된 문제:', {
    quizTitle: quizTitle.value,
    mainCategory: mainCategory.value,
    subCategory: subCategory.value,
    question: question.value,
    correctAnswer: correctAnswer.value,
    explanation: explanation.value,
  });
};
</script>

<style scoped>
.custom-file-upload {
  position: relative;
  display: inline-block;
}

.custom-file-upload input[type='file'] {
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.custom-file-upload label {
  display: inline-block;
  padding: 10px 20px;
  background-color: #000;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.custom-file-upload label:hover {
  background-color: #0056b3;
}
.container {
  display: flex;
  flex-direction: column;
}

.styled-file-input {
  display: flex;
  align-items: center; /* 요소들을 가운데 정렬합니다 */
  gap: 16px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 16px;
  width: 400px;
}

.attachment-button {
  width: fit-content;
  padding: 16px;
  background-color: #191b27;
  border-radius: 12px;
  color: white;
  font-weight: bold;
  cursor: pointer;
}

input[type='file'] {
  display: none;
}

.attached-file {
  font-size: 16px;
  font-weight: bold;
  color: #999;
  text-align: center;
}
.registerbtn {
  width: fit-content;
  padding: 16px;
  background-color: #191b27;
  border-radius: 12px;
  color: white;
  font-weight: bold;
  cursor: pointer;
}
</style>
