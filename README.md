# NODE.JS
REAL TIME CHAT
REAL-TIME CHAT APPLICATION USING WEBSOCKETS: TECHNICAL REPORT
1. Introduction
With the continuous growth of digital communication, the need for real-time messaging solutions has become increasingly critical. From social networking sites to customer support systems, instant communication allows for seamless and efficient user experiences. A real-time chat application enables users to send and receive messages instantly, without having to refresh the web page. This real-time interaction can significantly enhance user engagement and provide a competitive edge for web applications.
This report outlines the development of a real-time chat application using WebSockets, PHP, JavaScript, HTML, and CSS. It explores the key components of such an application, including user authentication, real-time communication via WebSockets, message storage, multimedia support, and user presence indicators. The goal is to offer a fully functional, scalable, and secure messaging solution suitable for integration into larger systems.
2. Technology Stack Overview
To achieve real-time functionality and interactive features, the application leverages a combination of client-side and server-side technologies:
•	HTML/CSS: For the layout and styling of the user interface, including login/signup forms and chat components.
•	JavaScript: To handle user interactions and facilitate real-time messaging on the client side.
•	PHP: For server-side processing, handling user data, and managing database operations.
•	MySQL: A relational database system used to store user profiles, chat history, and other metadata.
•	WebSockets: A protocol enabling two-way, persistent communication between the client and the server.
3. System Design and Architecture
The real-time chat system is composed of the following modules:
•	User Authentication System: Allows users to register and log in securely.
•	Chat Interface: Provides the frontend for message composition, display, and media attachments.
•	WebSocket Server: Facilitates bi-directional communication in real time.
•	Database Schema: Stores messages, user information, and media references.
•	Auxiliary Features: Includes emoji support, image and video sharing, user presence detection, and message deletion.
4. Implementation Steps
Step 1: Designing the Sign-Up and Login Forms
The first component is the user authentication system. Users can register by filling out a sign-up form that collects essential details such as first name, last name, email, password, and profile picture.
<form action="signup.php" method="post" enctype="multipart/form-data">
  <input type="text" name="fname" placeholder="First Name" required>
  <input type="text" name="lname" placeholder="Last Name" required>
  <input type="email" name="email" placeholder="Email" required>
  <input type="password" name="password" placeholder="Password" required>
  <input type="file" name="profile_pic">
  <button type="submit">Sign Up</button>
</form>
Login form example:
<form action="login.php" method="post">
  <input type="email" name="email" placeholder="Email" required>
  <input type="password" name="password" placeholder="Password" required>
  <button type="submit">Login</button>
</form>
Step 2: Backend Logic for Authentication
PHP scripts handle user registration and login. Passwords are securely hashed before storage. Sessions or tokens are used to manage user sessions.
$password = password_hash($_POST['password'], PASSWORD_DEFAULT);
Step 3: Database Design
Tables include:
•	users: Stores user details (id, name, email, hashed password, profile image).
•	messages: Stores chat messages (id, sender_id, receiver_id, message, timestamp, status).
•	media: Optional table for storing references to image/video files.
Step 4: Setting Up the WebSocket Server
We use the Ratchet PHP library to implement the WebSocket server. This server listens for client connections and routes messages appropriately.
use Ratchet\MessageComponentInterface;
use Ratchet\ConnectionInterface;

class Chat implements MessageComponentInterface {
    protected $clients;

    public function __construct() {
        $this->clients = new \SplObjectStorage;
    }

    public function onOpen(ConnectionInterface $conn) {
        $this->clients->attach($conn);
    }

    public function onMessage(ConnectionInterface $from, $msg) {
        foreach ($this->clients as $client) {
            $client->send($msg);
        }
    }

    public function onClose(ConnectionInterface $conn) {
        $this->clients->detach($conn);
    }
}
Run the WebSocket server:
php bin/chat-server.php
Step 5: Client-Side JavaScript for Messaging
JavaScript connects to the WebSocket server and handles message sending and receiving.
let socket = new WebSocket("ws://localhost:8080");

socket.onmessage = function(event) {
  let message = event.data;
  document.querySelector(".chat-box").innerHTML += `<div>${message}</div>`;
};

function sendMessage() {
  let msg = document.getElementById("message").value;
  socket.send(msg);
}
5. Key Features of the Application
•	Instant Messaging: Messages are exchanged instantly without refreshing the page.
•	Media Sharing: Users can upload and send images or videos.
•	User Presence: Online and offline statuses are updated in real time.
•	Message Deletion: Sent messages can be deleted. Deleted messages show a placeholder.
•	Typing Indicators: Optional feature to show when a user is typing.
•	Notification Sounds: Alert users to new messages.
•	Search Users: Find and initiate chats with registered users.
6. Security Considerations
•	Passwords are encrypted using bcrypt hashing.
•	All inputs are sanitized to prevent SQL injection and XSS.
•	WebSocket communication is secured using WSS for production.
•	CSRF tokens are used for authenticated requests.
7. Performance and Scalability
•	The use of WebSockets reduces server load compared to polling-based solutions.
•	Message queuing systems can be integrated to handle large-scale deployments.
•	Caching and database indexing improve performance.
8. Testing and Debugging
•	Unit tests for backend logic.
•	Frontend testing with tools like Selenium.
•	Real-time message testing with multiple browser sessions.
9. Screenshots and User Interface
Screenshots should illustrate:
 
Fig1: Sign-Up and Login Pages
 
Fig 2:Chat Interface with Real-Time Messages
 
Fig 3: Media Upload and Display
 
Fig 4: User Status Indicators
 
Fig 5: Message Delete and Edit Options
10. Conclusion
The development of a real-time chat application using WebSockets, PHP, JavaScript, and MySQL showcases the potential of modern web technologies to create responsive, dynamic, and user-friendly platforms. This application not only supports text messaging but also multimedia features, user presence detection, and message management functionalities. With further enhancements such as end-to-end encryption and mobile responsiveness, this chat system can serve as a robust communication tool for diverse web-based environments.
The integration of WebSockets significantly improves performance and user experience by facilitating persistent, low-latency connections. This positions the application as a strong foundation for any real-time communication needs in both social and enterprise contexts.
Build by Group 
