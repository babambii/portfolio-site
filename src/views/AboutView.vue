<template>
  <div class="about">
    <h1>AI関連情報</h1>
    <button @click="fetchAINews">情報を取得</button>
    
    <div v-if="loading" class="loading">
      読み込み中...
    </div>
    
    <div v-if="error" class="error">
      エラー: {{ error }}
    </div>
    
    <div v-if="articles.length > 0" class="articles">
      <div v-for="article in articles" :key="article.url" class="article-card">
        <h3>{{ article.title }}</h3>
        <p>{{ article.description }}</p>
        <small>出典: {{ article.source.name }}</small>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

interface Article {
  title: string;
  description: string;
  url: string;
  source: {
    name: string;
  };
}

interface NewsResponse {
  articles: Article[];
}

export default Vue.extend({
  name: 'AboutView',
  data() {
    return {
      articles: [] as Article[],
      loading: false,
      error: '' as string,
    };
  },
  methods: {
    async fetchAINews(): Promise<void> {
      this.loading = true;
      this.error = '';
      this.articles = [];

      try {
        // NewsAPI を使用して AI 関連の最新情報を取得
        const apiKey = process.env.VUE_APP_NEWS_API_KEY;
        if (!apiKey) {
          throw new Error('API キーが設定されていません。.env ファイルを確認してください。');
        }
        const response = await fetch(
          `https://newsapi.org/v2/everything?q=AI+OR+artificial+intelligence&sortBy=publishedAt&language=ja&pageSize=10&apiKey=${apiKey}`
        );

        if (!response.ok) {
          throw new Error('API リクエストに失敗しました');
        }

        const data: NewsResponse = await response.json();
        this.articles = data.articles;

        if (this.articles.length === 0) {
          this.error = '情報が見つかりませんでした';
        }
      } catch (err) {
        this.error = err instanceof Error ? err.message : '不明なエラーが発生しました';
      } finally {
        this.loading = false;
      }
    },
  },
});
</script>

<style scoped>
button {
  padding: 10px 20px;
  margin: 20px 0;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #359970;
}

.loading {
  text-align: center;
  padding: 20px;
  color: #666;
}

.error {
  color: #d63031;
  padding: 10px;
  background-color: #ffebee;
  border-radius: 4px;
  margin: 10px 0;
}

.articles {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.article-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.article-card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.article-card h3 {
  margin-top: 0;
  color: #333;
}

.article-card p {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
}

.article-card small {
  color: #999;
}
</style>
