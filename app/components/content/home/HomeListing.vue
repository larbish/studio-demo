<template>
  <ULandingSection>
    <template #title>
      <ContentSlot name="title" />
    </template>

    <template #description>
      <ContentSlot
        name="description"
        unwrap="p"
      />
    </template>

    <UBlogList>
      <UBlogPost
        v-for="article in articles"
        :key="article._path"
        :to="article._path"
        :title="article.title"
        :description="article.description"
        :image="article.image"
        :date="new Date(article.date).toLocaleDateString('en', { year: 'numeric', month: 'short', day: 'numeric' })"
        :ui="{
          description: 'line-clamp-2'
        }"
      />
    </UBlogList>
  </ULandingSection>
</template>

<script lang="ts" setup>
import type { Article } from '~/types'

const { count } = defineProps({
  count: {
    type: Number,
    default: 3
  }
})

const { data: articles } = await useAsyncData('articles', () => queryContent<Article>('/articles')
  .limit(count)
  .sort({ date: -1 })
  .find()
)
</script>
