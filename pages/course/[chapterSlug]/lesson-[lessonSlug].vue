<template>
  <div>
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson.title }}</h2>

    <div class="flex space-x-4 mt-2 ,b-8">
      <NuxtLink v-if="lesson.sourceUrl" class="font-normal text-md text-gray-500" :to="lesson.sourceUrl">Download
        Source
        Code</NuxtLink>

      <NuxtLink class="font-normal text-md text-gray-500" :to="lesson.downloadUrl">Download Video</NuxtLink>
    </div>

    <VideoPlayer v-if="lesson.videoId" :videoId="`${lesson.videoId}`" />
    <p>{{ lesson.text }}</p>

    <LessonCompleteButton :model-value="isLessonComplete" @update:model-value="toggleComplete" />

  </div>
</template>

<script setup>


const courses = useCourse()
const route = useRoute()

const chapter = computed(() => courses.chapters.find(chapter => chapter.slug === route.params.chapterSlug))


const lesson = computed(() => chapter.value.lessons.find(lesson => lesson.slug === route.params.lessonSlug))


const title = computed(() => {
  return `${lesson.value.title} - ${courses.title}`
})

useHead({
  title
})

const progress = useLocalStorage('progress', [])


const isLessonComplete = computed(() => {
  if (!progress.value[chapter.value.number - 1]) {
    return false;
  }
  if (
    !progress.value[chapter.value.number - 1][
    lesson.value.number - 1
    ]
  ) {
    return false;
  }
  return progress.value[chapter.value.number - 1][
    lesson.value.number - 1
  ];
});
const toggleComplete = () => {
  if (!progress.value[chapter.value.number - 1]) {
    progress.value[chapter.value.number - 1] = [];
  }
  progress.value[chapter.value.number - 1][
    lesson.value.number - 1
  ] = !isLessonComplete.value;
};
</script>

