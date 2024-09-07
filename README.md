# ChatterBox

ChatterBox is a real-time chat application built using the MERN (MongoDB, Express, React, Node.js) stack and Socket.io. This app enables users to connect with others, exchange messages instantly, and provides real-time updates using Socket.io.

## Features
- **User Authentication**: Secure login and signup using JWT authentication.
- **Real-time Messaging**: Messages are exchanged in real-time using WebSockets (Socket.io).
- **One-on-One Chat**: Users can start private conversations with other users.
- **Typing Indicators**: Get notified when the other person is typing.
- **Responsive Design**: The app is fully responsive, providing a seamless experience across devices.
- **Secure Storage**: MongoDB is used for storing user data and chat history.
- **Group Chat Functionality**: Extend the chat feature to allow group conversations.
## Tech Stack
- **Frontend**: React.js, Chakra UI for UI components, CSS for custom styling.
- **Backend**: Node.js, Express.js for handling API requests and Socket.io for real-time communication.
- **Database**: MongoDB for storing user details, chats, and messages.
- **WebSocket**: Socket.io to handle real-time messaging and user notifications.

## Project Structure
- **Frontend**: The frontend is built using React and Chakra UI for component styling. The Vite build tool is used for faster bundling and development.
- **Backend**: The backend is built using Node.js and Express, with MongoDB as the database. Socket.io manages the WebSocket connections for real-time communication.
- **Authentication**: User authentication is done through JWT (JSON Web Tokens) for secure login and session management.

## Prerequisites
Before you begin, ensure you have the following installed:
- **Node.js**: v14.x or higher
- **MongoDB**: Either locally or via MongoDB Atlas
- **Vite**: Build tool for React (if running locally)
- **Socket.io**: For real-time communication

## Getting Started

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/your-username/chatterbox.git
   cd chatterbox
   ```

2. **Install Dependencies**:

   In the root directory (for backend):
   ```bash
   npm install
   ```

   In the `client` directory (for frontend):
   ```bash
   cd client
   npm install
   ```

3. **Set Up Environment Variables**:

   In the root directory, create a `.env` file with the following environment variables:

   ```
   NODE_ENV=development
   PORT=5000
   MONGO_URI=<Your MongoDB Connection URI>
   JWT_SECRET=<Your JWT Secret>
   ```

   In the `client` directory, create a `.env.local` file:

   ```
   REACT_APP_API_URL=http://localhost:5000/api
   ```

4. **Run the Application**:

   Start the backend server:
   ```bash
   npm run server
   ```

   Start the frontend:
   ```bash
   cd client
   npm run dev
   ```

   The frontend will be running at `http://localhost:3000`, and the backend server will be running at `http://localhost:5000`.

## Socket.io Integration
ChatterBox leverages Socket.io for real-time chat functionality. Users can:
- Join private chat rooms.
- Receive messages instantly without page reloads.
- See real-time typing indicators.

Socket.io is initialized in the backend server and listens for chat events like "message received" and "typing".

### Example Socket.io Code (Server):
```js
const io = require('socket.io')(server, {
  pingTimeout: 60000,
  cors: {
    origin: "http://localhost:3000",
  },
});

io.on('connection', (socket) => {
  socket.on('setup', (userData) => {
    socket.join(userData._id);
    socket.emit('connected');
  });

  socket.on('join chat', (room) => {
    socket.join(room);
  });

  socket.on('typing', (room) => socket.in(room).emit('typing'));
  socket.on('stop typing', (room) => socket.in(room).emit('stop typing'));

  socket.on('new message', (newMessageReceived) => {
    const chat = newMessageReceived.chat;
    chat.users.forEach(user => {
      if (user._id !== newMessageReceived.sender._id) {
        socket.in(user._id).emit('message received', newMessageReceived);
      }
    });
  });
});
```

## Screenshots
Here’s a preview of the app:

### Login Page:

### Chat Interface:

## Future Enhancements
- **Message Notifications**: Add push notifications for new messages.
- **Voice and Video Chat**: Integrate WebRTC for voice and video chat.

## Personal Experience

During the development of **ChatterBox**, I focused on building an intuitive and user-friendly chat platform. This project was part of my continued journey into full-stack development with the MERN stack, including mastering Socket.io for real-time features. Previously, I worked on an **Interest Tracker** app using React, which taught me how to quickly develop and debug web applications under tight deadlines. I also built **Nutri Bot**, an AI chatbot focusing on nutrition using the OpenAI API and Next.js, strengthening my ability to integrate APIs into React projects. These experiences have prepared me well to tackle real-world applications like **ChatterBox**.

