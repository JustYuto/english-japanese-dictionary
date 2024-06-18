<template>
  <div>
    <input v-model="searchQuery" placeholder="検索する" />
    <button @click="searchDictionary">検索</button>
    <div v-if="searchResults.length">
      <h1>結果</h1>
      <ul>
        <li v-for="(result, index) in searchResults" :key="index">
          {{ result }}
        </li>
      </ul>
    </div>
    <div v-else>一致する単語が存在しません。</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dictionary: {},
      searchQuery: "",
      searchResults: [],
    };
  },
  mounted() {
    fetch("/dictionary.txt")
      .then((response) => response.text()) //.text()を使用し、文字列として取得
      .then((data) => {
        const lines = data.split("\n"); //一行毎にsplit
        lines.forEach((line) => {
          const trimmedLine = line.trim(); // 余分な空白をtrim
          if (trimmedLine) {
            const parts = trimmedLine.split(/\s+/); // タブまたは複数のスペース毎に分割
            if (parts.length >= 2) {
              // 最低2つの部分があるか確認
              const english = parts[0].trim(); // 英単語部分の余分な空白を除去
              const japanese = parts.slice(1).join(" ").trim(); // 残りの部分を全て日本語訳として扱う
              this.dictionary[english] = japanese; //englishをprimal keyとし、該当する日本語訳を割り当てるy
            }
          }
        });
      });
  },
  methods: {
    searchDictionary() {
      this.searchResults = [];
      const wordLower = this.searchQuery.toLowerCase(); // 小文字に変換
      //辞書内のすべてのenglish wordsを一つずつ探索
      Object.keys(this.dictionary).forEach((word) => {
        // キーワードに一致するかチェック
        if (word.toLowerCase().includes(wordLower)) {
          this.searchResults.push(word + ": " + this.dictionary[word]);
        }
        //word=英語
        //this.dictionary[word]=英語をkeyとした日本語
      });
    },
  },
};
</script>
