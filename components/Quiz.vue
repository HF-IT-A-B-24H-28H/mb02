<script setup lang="ts">
import { computed, ref, useId } from 'vue'
import { useNav } from '@slidev/client'

const props = defineProps<{
  question: string
  options: string[]
  answer: number
  explanation: string
}>()

// State belongs to this question, never to another slide or another learner.
const selected = ref<number | null>(null)
const groupName = useId()
const { isPrintMode } = useNav()
const revealed = ref(false)
const checked = ref(false)
const correct = computed(() => selected.value === props.answer)
const letter = (index: number) => String.fromCharCode(65 + index)

function reset() {
  selected.value = null
  revealed.value = false
  checked.value = false
}
</script>

<template>
  <section class="quiz" :class="{ exporting: isPrintMode }" @keydown.stop @click.stop @pointerdown.stop>
    <fieldset>
      <legend>{{ question }}</legend>
      <label v-for="(option, index) in options" :key="index" class="quiz-option"
        :class="{ chosen: selected === index, solution: revealed && index === answer }">
        <input type="radio" :name="groupName" :checked="selected === index" :disabled="revealed"
          :aria-label="`${letter(index)}: ${option}`"
          @change="selected = index; checked = false">
        <span><strong>{{ letter(index) }}.</strong> {{ option }}</span>
      </label>
    </fieldset>
    <div class="quiz-controls">
      <button :disabled="selected === null || revealed" @click="checked = true; revealed = correct">Prüfen</button>
      <button :disabled="revealed" @click="revealed = true">Auflösen</button>
      <button @click="reset">Zurücksetzen</button>
    </div>
    <div class="quiz-feedback" aria-live="polite" role="status">
      <p v-if="checked && !correct && !revealed">Noch nicht richtig. Diskutiert eure Begründung und versucht es erneut.</p>
      <p v-if="revealed"><strong>Lösung {{ letter(answer) }}.</strong> {{ explanation }}</p>
    </div>
    <div class="quiz-print"><strong>Lösung {{ letter(answer) }}.</strong> {{ explanation }}</div>
  </section>
</template>

<style scoped>
.quiz { font-size: 20px; line-height: 1.35; }
fieldset { border: 0; padding: 0; margin: 0; min-width: 0; }
legend { font-size: 23px; font-weight: 700; margin-bottom: 14px; }
.quiz-option { display: flex; gap: 12px; align-items: center; border: 2px solid #d4d4d6; border-radius: 3px; padding: 7px 13px; margin-bottom: 7px; background: #fdfcff; cursor: pointer; }
.quiz-option.chosen { border-color: #c54b13; background: #fff2eb; }
.quiz-option.solution { border-color: #15803d; background: #f0fdf4; }
input { width: 18px; height: 18px; flex-shrink: 0; accent-color: #c54b13; }
.quiz-controls { display: flex; gap: 12px; margin-top: 12px; }
button { border: 1px solid #c54b13; border-radius: 3px; padding: 7px 13px; color: #a63b0d; background: #fff; font: inherit; cursor: pointer; }
button:disabled { opacity: .45; cursor: default; }
button:focus-visible, .quiz-option:focus-within { outline: 3px solid #009dd5; outline-offset: 2px; }
.quiz-feedback { margin-top: 12px; min-height: 60px; font-size: 18px; }
.quiz-feedback p { margin: 0; }
.quiz-print { display: none; font-size: 17px; margin-top: 12px; }
.quiz.exporting .quiz-controls, .quiz.exporting .quiz-feedback { display: none; }
.quiz.exporting .quiz-print { display: block; }
@media print {
  .quiz-controls, .quiz-feedback { display: none; }
  .quiz-print { display: block; }
}
</style>
