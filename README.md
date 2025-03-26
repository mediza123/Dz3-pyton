<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Users and Posts</title>
  <style>
    .user-card {
      cursor: pointer;
      margin: 10px;
      padding: 10px;
      background-color: #f0f0f0;
      border-radius: 5px;
    }
    .post-card {
      border: 1px solid #ccc;
      margin: 10px;
      padding: 15px;
      border-radius: 5px;
      width: 300px;
    }
    .card-header {
      font-weight: bold;
      margin-bottom: 10px;
    }
    .card-body {
      margin-top: 10px;
    }
    #posts-container {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Users</h1>
  <div id="users-container"></div>
  <div id="posts-container"></div>

  <script>
    async function fetchData() {
      try {
        // Получаем данные пользователей и постов
        const usersResponse = await fetch('https://jsonplaceholder.typicode.com/users');
        const postsResponse = await fetch('https://jsonplaceholder.typicode.com/posts');
        
        const users = await usersResponse.json();
        const posts = await postsResponse.json();

        // Функция для отображения пользователей
        function displayUsers() {
          const usersContainer = document.getElementById('users-container');
          usersContainer.innerHTML = ''; // Очищаем контейнер

          users.forEach(user => {
            const userCard = document.createElement('div');
            userCard.classList.add('user-card');
            userCard.innerHTML = user.name;

            // Обработчик клика, чтобы отобразить посты пользователя
            userCard.addEventListener('click', () => displayPosts(user.id));

            usersContainer.appendChild(userCard);
          });
        }

        // Функция для отображения постов
        function displayPosts(userId) {
          const postsContainer = document.getElementById('posts-container');
          postsContainer.innerHTML = ''; // Очищаем контейнер

          const userPosts = posts.filter(post => post.userId === userId);

          userPosts.forEach(post => {
            const postCard = document.createElement('div');
            postCard.classList.add('post-card');
            postCard.innerHTML = `
              <div class="card-header">${post.title}</div>
              <div class="card-body">${post.body}</div>
            `;
            postsContainer.appendChild(postCard);
          });
        }

        // Отображаем список пользователей
        displayUsers();

      } catch (error) {
        console.error('Error fetching data:', error);
      }
    }

    fetchData();
  </script>
</body>
</html>
