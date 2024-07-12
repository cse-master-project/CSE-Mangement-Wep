<template>
  <q-form class="q-pa-md">
    <q-card>
      <q-p>단답형</q-p>
      <q-card-section>
        <q-select
          v-model="subject"
          :options="subjectOptions"
          label="과목"
          outlined
          class="q-mb-md"
          @update:model-value="updateDetailSubjectOptions"
        />
        <q-select
          v-model="detailSubject"
          :options="filteredDetailSubjectOptions"
          label="챕터"
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
        <!--첨부파일-->
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
  <!-- SubmitQuizSuccess 컴포넌트 -->
  <SubmitQuizSuccess
    v-if="submitQuizSuccess"
    :submit-quiz-success="submitQuizSuccess"
  />
</template>

<script setup>
import { ref, defineEmits, onMounted } from 'vue';
import { api } from 'src/boot/axios';
import SubmitQuizSuccess from 'src/components/quiz/SubmitQuizSuccess.vue';
import useCategories from 'src/services/useCategories.js';

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

const { subjectOptions, fetchCategories, getDetailSubjectsBySubject } =
  useCategories();

onMounted(fetchCategories);

const subject = ref('');
const detailSubject = ref('');
const quiz = ref('');
const answer = ref('');
const commentary = ref('');

const filteredDetailSubjectOptions = ref([]);

// 대분류 선택에 따라 소분류 옵션을 업데이트하는 함수
const updateDetailSubjectOptions = () => {
  filteredDetailSubjectOptions.value = getDetailSubjectsBySubject(
    subject.value,
  );
};

const submitQuizSuccess = ref(false);

// 서버에 문제 제출.
const submitQuiz = () => {
  const quizData = {
    subject: subject.value,
    detailSubject: detailSubject.value,
    quizType: '2',
    jsonContent: JSON.stringify({
      quiz: quiz.value,
      answer: answer.value,
      commentary: commentary.value,
    }),
    hasImage: false,
  };
  console.log('서버에 제출될 데이터:', quizData);
  api
    .post('/api/quiz/user', quizData)
    .then(response => {
      // console.log('서버 응답:', response.data);
      submitQuizSuccess.value = true;
    })
    .catch(error => {
      //console.error('서버 응답 오류:', error);
      if (error.response.status === 400) {
        // 예: 사용자에게 문제가 부족하거나 잘못된 데이터를 입력했다고 알림
        alert(
          '입력된 데이터가 부족하거나 잘못되었습니다. 빈칸이 없는지 확인해주세요 ^_^',
        );
      } else if (error.response.status === 500) {
        // 예: 서버 측에서 처리 중 오류 발생
        alert(
          '서버에서 문제를 처리하는 중 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.',
        );
      } else {
        // 기타 다른 오류 상황에 대한 처리
        alert('문제 등록 중 예상치 못한 오류가 발생했습니다.');
      }
      // 실패 시 사용자 경험을 개선할 수 있는 추가적인 로직 추가
    });
};
</script>

<style scoped lang="scss">
@import '/src/css/QuizBtn.css';

.textbox {
  font-family: 'Arial', sans-serif;
}
</style>