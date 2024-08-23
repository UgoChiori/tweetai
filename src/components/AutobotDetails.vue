   <template>
    <div class="max-w-2xl mx-auto p-4 bg-cover bg-center "
    
    >
      <h1 class="text-3xl text-pink-700 font-normal text-center mb-6">Autobots 🤖</h1>
      <ul class="space-y-4">
        <li
          v-for="autobot in autobots"
          :key="autobot.id"
          class="p-4 bg-pink shadow rounded-lg"
        >
          <div class="flex justify-between items-center">
            <span class="text-xl text-pink-700 font-normal">{{ autobot.name }}</span>
            <button
              @click="togglePosts(autobot.id)"
              :class="{
                'bg-pink-700 hover:bg-pink-400': selectedAutobotId !== autobot.id,
                'bg-red-500 hover:bg-red-600': selectedAutobotId === autobot.id,
              }"
              class="text-white px-4 py-2 rounded transition-colors duration-300 ease-in-out"
            >
              {{ selectedAutobotId === autobot.id ? 'Hide Posts' : 'Show Posts' }}
            </button>
          </div>
          <ul v-if="selectedAutobotId === autobot.id" class="mt-4 space-y-2">
            <li
              v-for="post in posts"
              :key="post.id"
              class="p-4 bg-pink border border-[pink-700] rounded-lg"
            >
              <div class="flex justify-between items-center">
                <span class="font-normal text-pink-700">{{ post.title }}</span>
                <button
                  @click="toggleComments(post.id)"
                  :class="{
                    'bg-pink-700 hover:bg-pink-400': selectedPostId !== post.id,
                    'bg-red-500 hover:bg-red-600': selectedPostId === post.id,
                  }"
                  class="text-white px-4 py-2 rounded transition-colors duration-300 ease-in-out"
                >
                  {{ selectedPostId === post.id
                    ? 'Hide Comments'
                    : 'Show Comments' }}
                </button>
              </div>
              <ul v-if="selectedPostId === post.id" class="mt-3 space-y-2 pl-4  borde">
                <li
                  v-for="comment in uniqueComments"
                  :key="comment.id"
                  class="pl-2 text-pink-700"
                >
                  {{ truncateComment(comment.body) }}
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
  import { ToastProps, useToast } from "vue-toast-notification";

  const toast = useToast();
  
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
  
      const fetchPosts = async (autobotId: number): Promise<void> => {
        try {
          const response = await axios.get(
            `http://localhost:2000/autobots/${autobotId}/posts`
          );
          posts.value = response.data;
          selectedPostId.value = null;
          comments.value = [];
        } catch (error: any) {
          if (error.response && error.response.status === 429) {
            toast.error("Too many requests. Please try again after a minute.");
          } else {
           toast.error("Error fetching posts:", error);
          }
        }
      };
  
    

      const fetchComments = async (postId: number) => {
        try {
          const response = await axios.get(
            `http://localhost:2000/posts/${postId}/comments`
          );
          const newComments = response.data;
          // Prevent duplicate comments
          comments.value = newComments.filter(
            (newComment: { id: number; }) =>
              !comments.value.some(
                (existingComment) => existingComment.id === newComment.id
              )
          );
        } catch (error) {
          toast.error("Error fetching comments:", error as ToastProps);
        }
      };
  
      const togglePosts = (autobotId: number) => {
        if (selectedAutobotId.value === autobotId) {
          selectedAutobotId.value = null;
          posts.value = [];
        } else {
          selectedAutobotId.value = autobotId;
          fetchPosts(autobotId);
        }
      };
  
      const toggleComments = (postId: number) => {
        if (selectedPostId.value === postId) {
          selectedPostId.value = null;
          comments.value = [];
        } else {
          selectedPostId.value = postId;
          fetchComments(postId);
        }
      };
  
      const truncateComment = (comment: string) => {
        return comment.length > 50 ? comment.substring(0, 50) + "..." : comment;
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
        togglePosts,
        toggleComments,
        truncateComment,
        uniqueComments: comments, // Use filtered unique comments
      };
    },
  };
  </script>
  
  <style scoped>
 
  /* Add styles here if needed */
  </style>
  