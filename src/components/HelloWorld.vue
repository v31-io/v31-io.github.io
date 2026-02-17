<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card";
import { ExternalLink } from "lucide-vue-next";
import { siGithub } from "simple-icons";

interface Repository {
  id: number;
  name: string;
  description: string | null;
  html_url: string;
  homepage: string | null;
}

defineProps<{ msg: string }>();

const repos = ref<Repository[]>([]);
const loading = ref(true);

const fetchRepos = async () => {
  try {
    const response = await axios("https://api.github.com/orgs/v31-dev/repos");
    repos.value = response.data;
  } catch (error) {
    console.error("Error fetching repos:", error);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchRepos();
});
</script>

<template>
  <div class="w-full min-h-screen bg-background text-foreground p-8">
    <div class="max-w-6xl mx-auto">
      <h1 class="text-4xl font-bold mb-8 text-center">{{ msg }}</h1>

      <div v-if="loading" class="text-center py-12">
        <p class="text-muted-foreground">Loading repositories...</p>
      </div>

      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <Card
          v-for="repo in repos"
          :key="repo.id"
          class="bg-card border-border hover:border-primary transition-colors"
        >
          <CardHeader>
            <div class="flex items-start justify-between">
              <CardTitle class="text-xl text-foreground">{{
                repo.name
              }}</CardTitle>
              <div class="flex gap-2">
                <a
                  v-if="repo.html_url"
                  :href="repo.html_url"
                  target="_blank"
                  rel="noopener"
                  class="text-muted-foreground hover:text-foreground transition-colors"
                  title="View on GitHub"
                >
                  <div class="w-5 h-5 [&>svg]:fill-current" v-html="siGithub.svg"></div>
                </a>
                <a
                  v-if="repo.homepage"
                  :href="repo.homepage"
                  target="_blank"
                  rel="noopener"
                  class="text-muted-foreground hover:text-foreground transition-colors"
                  title="Visit homepage"
                >
                  <ExternalLink :size="20" />
                </a>
              </div>
            </div>
          </CardHeader>
          <CardContent>
            <p class="text-muted-foreground text-sm">
              {{ repo.description || "No description available" }}
            </p>
          </CardContent>
        </Card>
      </div>
    </div>
  </div>
</template>
