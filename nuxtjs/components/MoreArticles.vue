<template>
  <section class="more-articles">
    <div class="container">
      <h1 class="more-articles__title">More Articles</h1>
      <div v-if="articles.length !== 0" class="articles-grid">
        <Article
          v-for="(article, index) in articles"
          :key="index"
          :article="article"
          :bordered="index !== articles.length - 1"
        />
      </div>
    </div>
  </section>
</template>

<script>
import { formatRelativeTime } from "../../shared/utils/format-relative-time";

export default {
  data() {
    return {
      articles: []
    };
  },
  async fetch() {
    const response = await this.$directus.items("articles").readMany({
      fields: ["*", "author.avatar", "author.first_name", "author.last_name"],
      limit: 2
    });
    const formattedArticles = response.data.map(article => {
      return {
        ...article,
        publish_date: formatRelativeTime(new Date(article.publish_date))
      };
    });
    this.articles = formattedArticles;
  }
};
</script>
