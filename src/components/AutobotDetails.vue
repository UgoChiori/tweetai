<!-- 
<template>
  <div>
    <h1>Autobots</h1>
    <ul>
      <li v-for="autobot in autobots" :key="autobot.id">
        {{ autobot.name }}
        <button @click="fetchPosts(autobot.id)">Show Posts</button>
        <ul v-if="selectedAutobotId === autobot.id">
          <li v-for="post in posts" :key="post.id">
            {{ post.title }}
            <button @click="fetchComments(post.id)">Show Comments</button>
            <ul v-if="selectedPostId === post.id">
              <li v-for="comment in comments" :key="comment.id">
                {{ comment.body }}
              </li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

interface Autobot {
  id: number;
  name: string;
  username: string;
  email: string;
}

interface Post {
  id: number;
  title: string;
  body: string;
  autobot_id: number;
}

interface Comment {
  id: number;
  body: string;
  post_id: number;
  name: string;
  email: string;
}

export default {
  name: 'AutobotList',
  setup() {
    const autobots = ref<Autobot[]>([]);
    const posts = ref<Post[]>([]);
    const comments = ref<Comment[]>([]);
    const selectedAutobotId = ref<number | null>(null);
    const selectedPostId = ref<number | null>(null);

    const fetchAutobots = async () => {
      try {
        const response = await axios.get('http://localhost:2000/autobots');
        autobots.value = response.data;
      } catch (error) {
        console.error('Error fetching Autobots:', error);
      }
    };

    const fetchPosts = async (autobotId: number): Promise<void> => {
      try {
        selectedAutobotId.value = autobotId;
        const response = await axios.get(`http://localhost:2000/autobots/${autobotId}/posts`);
        posts.value = response.data;
        selectedPostId.value = null;
        comments.value = [];
      } catch (error) {
        console.error('Error fetching posts:', error);
      }
    };

    const fetchComments = async (postId: number) => {
      try {
        selectedPostId.value = postId;
        const response = await axios.get(`http://localhost:2000/posts/${postId}/comments`);
        comments.value = response.data;
      } catch (error) {
        console.error('Error fetching comments:', error);
      }
    };

    onMounted(() => {
      fetchAutobots();
    });

    return {
      autobots,
      posts,
      comments,
      selectedAutobotId,
      selectedPostId,
      fetchPosts,
      fetchComments,
    };
  },
};
</script>

<style scoped>
/* Add styles here if needed */
</style>
 -->


 <template>
  <div class="max-w-2xl mx-auto p-4">
    <h1 class="text-3xl font-bold text-center mb-6">Autobots</h1>
    <ul class="space-y-4">
      <li
        v-for="autobot in autobots"
        :key="autobot.id"
        class="p-4 bg-white shadow rounded"
      >
        <div class="flex justify-between items-center">
          <span class="text-xl font-medium">{{ autobot.name }}</span>
          <button
            @click="fetchPosts(autobot.id)"
            class="bg-blue-500 text-white px-3 py-1 rounded hover:bg-blue-600"
          >
            Show Posts
          </button>
        </div>
        <ul v-if="selectedAutobotId === autobot.id" class="mt-4 space-y-2">
          <li
            v-for="post in posts"
            :key="post.id"
            class="p-3 bg-gray-100 rounded"
          >
            <div class="flex justify-between items-center">
              <span>{{ post.title }}</span>
              <button
                @click="fetchComments(post.id)"
                class="bg-green-500 text-white px-3 py-1 rounded hover:bg-green-600"
              >
                Show Comments
              </button>
            </div>
            <ul v-if="selectedPostId === post.id" class="mt-2 space-y-1">
              <li
                v-for="comment in comments"
                :key="comment.id"
                class="pl-4 border-l-2 border-green-500"
              >
                {{ comment.body }}
              </li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";

interface Autobot {
  id: number;
  name: string;
  username: string;
  email: string;
}

interface Post {
  id: number;
  title: string;
  body: string;
  autobot_id: number;
}

interface Comment {
  id: number;
  body: string;
  post_id: number;
  name: string;
  email: string;
}

export default {
  name: "AutobotList",
  setup() {
    const autobots = ref<Autobot[]>([]);
    const posts = ref<Post[]>([]);
    const comments = ref<Comment[]>([]);
    const selectedAutobotId = ref<number | null>(null);
    const selectedPostId = ref<number | null>(null);

    const fetchAutobots = async () => {
      try {
        const response = await axios.get("http://localhost:2000/autobots");
        autobots.value = response.data;
      } catch (error) {
        console.error("Error fetching Autobots:", error);
      }
    };

    const fetchPosts = async (autobotId: number) => {
  try {
    selectedAutobotId.value = autobotId;
    const response = await axios.get(`http://localhost:2000/autobots/${autobotId}/posts`);
    posts.value = response.data;
    selectedPostId.value = null;
    comments.value = [];
  } catch (error: any) {
    if (error.response && error.response.status === 429) {
      alert('Too many requests. Please try again after a minute.');
    } else {
      console.error('Error fetching posts:', error);
    }
  }
};
    // const fetchPosts = async (autobotId: number) => {
    //   try {
    //     selectedAutobotId.value = autobotId;
    //     const response = await axios.get(
    //       `http://localhost:2000/autobots/${autobotId}/posts`
    //     );
    //     posts.value = response.data;
    //     selectedPostId.value = null;
    //     comments.value = [];
    //   } catch (error) {
    //     console.error("Error fetching posts:", error);
    //   }
    // };

    const fetchComments = async (postId: number) => {
      try {
        selectedPostId.value = postId;
        const response = await axios.get(
          `http://localhost:2000/posts/${postId}/comments`
        );
        comments.value = response.data;
      } catch (error) {
        console.error("Error fetching comments:", error);
      }
    };

    onMounted(() => {
      fetchAutobots();
    });

    return {
      autobots,
      posts,
      comments,
      selectedAutobotId,
      selectedPostId,
      fetchPosts,
      fetchComments,
    };
  },
};
</script>

<style scoped>
/* Add styles here if needed */
</style>
