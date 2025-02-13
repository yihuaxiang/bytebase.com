<template>
  <main class="overflow-hidden space-y-8">
    <img
      class="hidden sm:block sm:h-96 w-full object-scale-down"
      :src="post.feature_image"
      :alt="post.feature_image_alt"
    />
    <div class="prose prose-xl md:prose-2xl mx-auto px-4">
      <div
        v-for="(tag, tagIndex) in post.tags"
        :key="tagIndex"
        class="mb-4 inline-flex"
      >
        <span
          class="items-center px-3 py-0.5 mr-2 rounded-full text-base font-medium"
          :class="getTagStyle(tag.name)"
        >
          {{ tag.name }}
        </span>
      </div>
      <h1>{{ post.title }}</h1>
    </div>
    <span
      class="flex flex-row px-2 items-center justify-center block text-base text-gray-900 font-semibold tracking-wide uppercase"
    >
      <img
        class="h-10 w-10 rounded-full mr-2"
        :src="post.authors[0].profile_image"
        alt=""
      />{{ post.authors[0].name }}
      <div class="ml-2 flex space-x-1 text-gray-500">
        <time :datetime="post.published_at">
          {{
            new Date(post.published_at).toLocaleString("default", {
              year: "numeric",
              month: "short",
              day: "numeric",
            })
          }}
        </time>
        <span aria-hidden="true">
          &middot;
        </span>
        <span> {{ post.reading_time }} min read </span>
      </div></span
    >
    <div
      class="prose prose-indigo prose-xl md:prose-2xl mx-auto px-4"
      v-html="post.html"
    ></div>

    <div class="border max-w-xl mx-auto px-4 sm:px-6 lg:px-8 lg:max-w-5xl">
      <ActionSection class="sm:justify-center" :moduleName="'blog-detail'" />
    </div>
  </main>
</template>

<script lang="ts">
import { PostTag, postTagStyle } from "../../../common/type";
import { getSinglePost } from "../../../api/posts";

export default {
  head() {
    return {
      title: (this as any).post.title,
    };
  },
  // Have to use asyncData, CompositionAPI useAsync on the other hand doesn't refresh after first load.
  async asyncData({ params }: any) {
    const post = await getSinglePost(params.slug);
    return { post: post };
  },
  methods: {
    getTagStyle(tag: PostTag): string {
      return postTagStyle(tag);
    },
  },
};
</script>
