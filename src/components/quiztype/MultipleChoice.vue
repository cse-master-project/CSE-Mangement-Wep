<template>
  <q-form class="q-pa-md">
    <q-card>
      <q-card-section>
        <q-select
          v-model="subject"
          :options="subjectOptions"
          label="대분류"
          outlined
          class="q-mb-md"
        />
        <q-select
          v-model="detailSubjet"
          :options="detailSubjectOptions"
          label="소분류"
          outlined
          class="q-mb-md"
        />
        <q-input
          v-model="quiz"
          type="textarea"
          outlined
          rows="4"
          placeholder="문제를 입력해주세요"
          maxlength="300"
          class="q-mb-md"
        />

        <div
          v-for="index in 4"
          :key="`choice-${index}`"
          class="choice-container"
        >
          <q-input
            v-model="option[index - 1].label"
            type="textarea"
            :label="'보기 ' + index"
            outlined
            autogrow
            style="margin: 10px 0"
            class="q-mb-md"
          />
        </div>

        <q-input
          v-model="answer"
          type="textarea"
          label="정답"
          outlined
          autogrow
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
          style="width: 10%; margin: 3% 1%"
          >뒤로가기</q-btn
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
import { api } from 'src/boot/axios';
import { useManagerStore } from 'src/stores/auth';

const emits = defineEmits(['change-quiz-type']);
const goBack = () => {
  emits('change-quiz-type', '');
};

const fileName = ref('');
const fileInputHandler = event => {
  const files = event.target && event.target.files;
  if (files && files[0]) {
    fileName.value = event.target.files[0].name;
  }
};

const subjectOptions = [
  { label: 'c언어', value: 'C' },
  { label: '파이썬', value: 'Python' },
  { label: '자료구조', value: 'Data structure' },
];

const detailSubjectOptions = [
  { label: '스택', value: 'Stack' },
  { label: '큐', value: 'Queue' },
  { label: '그래프', value: 'Graph' },
];

const subject = ref('');
const detailSubjet = ref('');
const quiz = ref('');
const option = ref([
  { label: '', value: '1' },
  { label: '', value: '2' },
  { label: '', value: '3' },
  { label: '', value: '4' },
]);
const answer = ref('');
const commentary = ref('');

// watch(
//   option,
//   newChoices => {
//     // newChoices.forEach((choice, index) => {
//     //   choice.label = `${index + 1}번 보기`;
//     // });
//   },
//   { deep: true, immediate: true },
// );

const submitQuiz = () => {
  const quizData = {
    subject: subject.value,
    detailSubject: detailSubjet.value,
    jsonContent: JSON.stringify({
      type: '1',
      quiz: quiz.value,
      option: option.value.map(choice => choice.label),
      answer: answer.value,
      commentary: commentary.value,
    }),
    hasImage: false,
  };
  console.log('서버에 제출될 데이터:', quizData);
  const managerStore = useManagerStore();
  const accessToken = managerStore.accessToken;

  api
    .post('/api/quiz/default', quizData, {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    })
    .then(response => {
      console.log('서버 응답:', response.data);
      // 성공적으로 서버에 데이터를 전송한 후의 동작
    })
    .catch(error => {
      console.error('서버 응답 오류:', error);
      // 서버에 데이터 전송 중 오류가 발생한 경우의 동작
    });
};
</script>

<style scoped>
@import '/src/css/QuizBtn.css';
</style>
