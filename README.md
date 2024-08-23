---

# TweetAI Frontend

Welcome to the TweetAI frontend! This project is a Vue.js application that interacts with a backend service to display and manage data related to Autobots, posts, and comments.

## Table of Contents

- [Project Overview](#project-overview)
- [Installation](#installation)
- [Usage](#usage)
- [Components](#components)
- [Styling](#styling)
- [Backend](#backend)
- [GitHub Repository](#github-repository)

## Project Overview

This Vue.js frontend application is designed to work with the TweetAI backend service. It features:

- A list of Autobots with the ability to fetch and display their posts.
- A detail view to show comments for each post.

## Installation

To set up this project locally, follow these steps:

1. **Clone the repository:**

   ```bash
   git clone https://github.com/UgoChiori/tweetai.git
   cd tweetai
   ```

2. **Install dependencies:**

   ```bash
   npm install
   ```

## Usage

To run the application locally, use the following command:

```bash
npm run dev
```

Visit `http://localhost:5173` in your browser to see the application in action.

## Components

### AutobotCount.vue

- Displays the total count of Autobots in the system.
- Fetches the count from the backend API and shows it in a simple interface.

### AutobotDetails.vue

- Lists all Autobots with a button to fetch and display their posts.
- Each post has a button to fetch and display comments associated with it.

## Styling

This project uses Tailwind CSS for styling. The configuration can be found in `tailwind.config.js`, and styles are applied directly in the component files using Tailwind's utility classes.

## Backend

The backend service for this application is hosted at [https://tweetai-backend-x755.vercel.app/](https://tweetai-backend-x755.vercel.app/).

For more information on the backend, visit the [GitHub repository](https://github.com/UgoChiori/tweetai-backend).

## Hosted Application
You can view the live application at https://tweetai-rahslzvhu-ugochioris-projects.vercel.app/.

# Note: The data may not display correctly due to limitations in the backend's MySQL hosting services, which are not free. This project is intended for testing purposes and may not show live data outside of localhost.

## GitHub Repository

For more information on the frontend project, visit the [GitHub repository](https://github.com/UgoChiori/tweetai).

Feel free to open issues or submit pull requests if you find bugs or want to contribute!

---
